# Generic DOM

### Summary:
This proposal outlines the addition of a Generic DOM API, which represents an in-memory representation of a document object model suitable for modeling data stored in forms analagous to JSON or XML documents.

The Generic DOM shall be usable as a drop-in replacement for a `rapidjson::GenericDocument` but with the added benefits of:
- Supporting in-memory representation of arbitrary types
- Supporting XML-style named node structures that have both a set of attributes and a set of values simultaneously
- Supporting serialization to arbitrary DOM formats: XML and JSON to start with, with opportunities to provide support to other formats such as YAML in the future.

### What is the relevance of this feature?
The Generic DOM is intended to align the backend representation and API for several existing and upcoming features that involve DOM manipulation, including:
- The JSON Serializer, which serializes reflected O3DE types to and from JSON
- Prefabs (currently using rapidjson for its own model and the O3DE JSON serializer for marshalling)
- The Document Property Editor (pending RFC, currently slated to use this generic DOM without a serialized representation)

Aligning on a common API allows shared tooling and serialization support. This is aimed towards avoiding duplication of effort by ensuring serialization and utility methods may be shared between all projects using the generic DOM. Additionally, by decoupling the DOM storage format from the underlying serialization, we gain flexibility in choosing the optimal data format to serialize to, instead of writing a bespoke solution for just a given format.

The Generic DOM aims to shoot for the following tenets:
- *Straightforward to use*. In particularly, the Generic DOM API is being designed such that most existing code using `rapidjson` can use the Generic DOM as a drop-in replacement.
- *Flexible*. The Generic DOM API is intended to enable extensions to be built as DOM manipulations without expanding the scope of the DOM API itself, and the DOM is designed to be straightforward to serialize to common document object model formats such as JSON, YAML, and XML.
- *Lightweight*. The Generic DOM API is only intended to fill in the minimum required set of functionality to provide a generic, format-agnostic DOM. All other functionality, such as serializing arbitrary formats, shall merely consume this API by way of composition.

The Generic DOM is not intended to be a runtime replacement for the binary serialization provided by O3DE's reflection system. Processed assets using O3DE reflection shall still allow for baking assets into an efficient, binary format. The Generic DOM is intended to be used either in tools at authoring time or for deserializing types that come from sources outside of the engine, such as JSON payloads from web services.

### Feature design description:
The Generic DOM shall consist of a Reader API that implements the [visitor pattern](https://en.wikipedia.org/wiki/Visitor_pattern) with several *backends*, an in-memory Document API, and a patch generation API.

The Visitor / Reader API shall support transforming DOM data from one format to another through a visiting mechanism in which each DOM element is composed into method calls listened to by backend formats that compose a transformation. An arbitrary value may be read from one backend format and written to another backend format. Backends shall be exposed for formats on an as-needed basis.

The initial backends shall include JSON and XML, with a visitor interface for visiting an in-memory Generic DOM as well.

The Document API shall expose a `Document` that is a handle to a top-level `Value`. `Value` is a variant type that stores one of the following types of values:
- Primitives: plain data types
  - int64
  - uint64
  - bool (represented as storing the "True" or "False" type)
  - double
  - null (this is the default for uninitialized values)
- Strings: references to string data - by default this is done without copying the string, but an interface will be provided to create an owning, ref-counted copy of the string. This copy will be immutable and thus safely sharable across thread boundaries.
- Object: JSON-style objects that represent a map of keys (stored as `AZ::Name` in this implementation) to `Value`s.
- Array: JSON-style arrays that represent an ordered sequence of `Value`s
- Node: XML-style nodes that have three components: a node name (`AZ::Name`), a map of keys to `Value`s, and an ordered sequence of `Value`s.
- Opaque: An arbitrary value stored in an `AZStd::any`. This is a non-serializable representation of an entry used only for in-memory options. This is intended to be used as an intermediate value over the course of DOM transformation and as a proxy to pass through types of which the DOM has no knowledge to other systems.

The Document API shall also expose a `Path` class that represents a vector of keys to provide a unique path within the DOM, similar to JSON-pointer that refers to a given `Value` in a `Document`. Their resolution rules will follow the JSON pointer standard, with a couple caveats:
- `Node` values shall be supported, and may be indexed into by either a numeric index or an attribute string name.
- Opaque value contents may not be indexed into in any way.

Serialization shall be handled by external serializer classes for each supported DOM format: JSON and XML to start with. `AZStd::any` shall not be supported directly for serialization, but a callback may be added to the serializers to attempt to resolve an `any` into another `Value` type (using the O3DE reflection system, for instance), if desired.

Serializers are not part of the core Generic DOM API. A registration system to map file formats to "default" format serializers that provide one possible mapping of a DOM to and from a given representation shall be provided, but other serializers may use the Visitor / Reader API to implement arbitrary format support.

`JsonBackend` shall be responsible for serialization into JSON and deserialziation out of JSON. All values shall use their canonical JSON representations except Nodes, which will be marked as only being supported as `Emulated` in the backend. Nodes shall be converted into Objects before being serialized. Upon deserialization, Nodes shall be brought in as Objects, but may be losslessly converted back to Nodes.

A node's `Emulated` object representation is comprised of a JSON object with a single key, its value an array comprised of an object containing all of the Node attributes (an empty object in a case where no attributes are specified), followed by the sequence of the node's children.

Here's an example of a `Value` JSON serialized using Node types:
```json
{
    "Reflection": [
        {
            "namespace": "LmbrCentral"
        },
        {
            "Include": {
                "path": "AzToolsFramework/ToolsComponents/EditorComponentBase.h",
                "in": "header"
            }
        },
        {
            "Include": {
                "path": "Source/Editor/EditorCommentComponent.reflection.h",
                "in": "body"
            }
        }
    ]
}
```
Versus its XML representation produced by XML serialization:
```xml
<Reflection namespace="LmbrCentral">
    <Include path="AzToolsFramework/ToolsComponents/EditorComponentBase.h" in="header" />
    <Include path="Source/Editor/EditorCommentComponent.reflection.h" in="body" />
</Reflection>
```

`XmlBackend` shall use be responsible for serialization into XML and deserialization out of XML. XML shall always serialize its root element as a Node and shall rely upon tags in the `o3de` namespace to support functionality that doesn't have a 1:1 XML correspondance:
- Root elements that aren't of the Node type shall be stored in an `o3de:Document` tag.
- Arrays and objects shall be stored in `o3de:Container` tags comprised of `o3de:MapEntry` or `o3de:ArrayEntry` tags homogenously corresponding to their value types. Containers may optionally have a `name` field that indicates the container is to be treated as an element with a given key in the node's attribute map instead of a sequential child of the parent node. Containers may be nested hierarchically, so long as their immediate ancestor is a tag within the `o3de` namespace to allow some degree of concision in nested container serialization.

Here's an abridged example of a `Value` XML serialized using some of these extension types:
```xml
<o3de:Document xmlns:o3de="https://o3de.org/">
    <o3de:Container>
        <o3de:MapEntry name="ContainerEntity">
            <o3de:MapEntry name="Id">ContainerEntity</o3de:MapEntry>
            <o3de:MapEntry name="Name">Pawn</o3de:MapEntry>
            <o3de:MapEntry name="Components">
                <o3de:MapEntry name="Component_[11484796288798653912]">
                    <o3de:MapEntry name="$type">EditorInspectorComponent</o3de:MapEntry>
                    <o3de:MapEntry name="Id">11484796288798653912</o3de:MapEntry>
                    <o3de:MapEntry name="ComponentOrderEntryArray">
                        <o3de:ArrayEntry>
                            <o3de:MapEntry name="ComponentId">16987082192253801011</o3de:MapEntry>
                        </o3de:ArrayEntry>
                    </o3de:MapEntry>
            </o3de:MapEntry>
        </o3de:MapEntry>
    </o3de:Container>
</o3de:Document>
```
Note: The o3de namespace and its corresponding tags are only required for DOMs that don't correspond 1:1 with an XML representation, as expressed above. A Generic DOM hierarchy expressed as Node types with no Objects or Arrays shall not have any of these `o3de:` tags at all and instead look like XML as dictated by their structure, as seen in the Reflection example above.

Versus its JSON representation:
```json
{
    "ContainerEntity": {
        "Id": "ContainerEntity",
        "Name": "Pawn",
        "Components": {
            "Component_[11484796288798653912]": {
                "$type": "EditorInspectorComponent",
                "Id": 11484796288798653912,
                "ComponentOrderEntryArray": [
                    {
                        "ComponentId": 16987082192253801011
                    }
                ]
            }
        }
    }
}
```

### Technical design description:
The visitor API, modeled after rapidjson's SAX streaming, allows consumers to adapt from a `VisitorInterface` to produce a DOM from a given backend. These visitors can be used to read data and be composed to move data into another format, be it an in-memory representation or an on-disk one.

Visitor API:
```cpp
struct VisitorInterface
{
    // All methods return true or false dependent on whether an operation was succesful.
    virtual bool Null();
    virtual bool Bool(bool);
    virtual bool Int(int64_t);
    virtual bool Uint(uint64_t);
    virtual bool Double(double);
    virtual bool String(const char* string, size_t length, bool copy);

    // The opaque type is not designed to be serialized and will be rejected by standard serializers
    virtual bool OpaqueValue(const AZStd::any& value) { return false; }

    virtual bool StartNode(const char* name, size_t length, bool copy);
    virtual bool EndNode(size_t attributeCount, size_t elementCount);

    virtual bool StartObject();
    virtual bool EndObject(size_t attributeCount);
    virtual bool Key(const char*, size_t length, bool copy);

    virtual bool StartArray();
    virtual bool EndArray(size_t elementCount);
};

//! Backends are registered centrally and used to transition DOM formats to and from a given file.
//! Other formats (such as direct in-memory representations) may be simply implemented as helper functions that
//! process data and return any intermediate value required.
class DomBackend
{
    //! Attempt to read this format from the given file into the target VisitorInterface.
    bool ReadFromPath(AZ::IO::PathView pathName, VisitorInterface* visitor);
    //! Attempt to read this format from the given stream into the target VisitorInterface.
    virtual bool ReadFromBuffer(const char* buffer, size_t length, bool copyStrings) = 0;
    //! Acquire a visitor interface for writing to the target output file.
    AZStd::unique_ptr<VisitorInterface> CreatePathWriter(AZ::IO::PathView pathName);
    //! Acquire a visitor interface for writing to the target output buffer.
    virtual AZStd::unique_ptr<VisitorInterface> CreatePathWriter(AZStd::vector<char>& outputBuffer) = 0;

    enum class TypeSupportLevel
    {
        //! Unsupported types shall be rejected by this backend.
        Unsupported,
        //! Native types can be serialized losslessly into this backend format.
        Native,
        //! Emulated types can be serialized into this backend format but they may be transformed
        //! into an intermediate representation first.
        Emulated,
    }

    //! Returns the extent to which this backend format supports a given type.
    //! Unsupported types may not be used and shall not be emitted from this backend.
    virtual TypeSupportLevel GetTypeSupportLevel(AZ::DOM::Type type) = 0;
};

//! Central interface for registering and looking up backend types by file extension.
class BackendRegistry
{
public:
    //! Registers a factory for a given backend.
    template <class TBackend>
    void RegisterBackend(AZ::Name name, AZStd::vector<AZStd::string> extensions);

    //! Looks up a DOM backend based on its name.
    DomBackend* GetBackendByName(AZ::Name name);

    //! Looks up a DOM backend based on a file extension.
    DomBackend* GetBackendForExtension(AZStd::string_view extension);

private:
    // <internal registration helpers here>
};
```

The Value and Document API is meant to be as close as possible to a drop-in replacement for rapidjson, with a couple important extnesions:
- Generic DOM values support "node" types - XML-esque data structures that have a name, a set of attributes, and an ordered sequence of values. Node types may be converted to and from object types losslessly, though objects must have a specific shape to be eligible for conversion to node types.
- Generic DOM values may store arbitrary non-serializable opaque values in the form of an `AZStd::any`. These can be used for in-memory DOM operations or as an intermediate step for DOM transformations in which a serializer converts these arbitrary types into a seriazable representation.

Value API:
```cpp
// Currently we're storing all keys as string types
// For JSON-style data, it may be desirable to also support numeric indexing
using KeyType = AZ::Name;

// Type entries match rapidjson, excepting kNodeType and kOpaqueType
enum class Type
{
    kNullType = 0,
    kFalseType = 1,
    kTrueType = 2,
    kObjectType = 3,
    kArrayType = 4,
    kStringType = 5,
    kNumberType = 6,
    kNodeType = 7,
    kOpaqueType = 8,
};

class Value
{
    using string = AZStd::string;
    using string_view = AZStd::string_view;

public:
    // Constructors...
    explicit Value();
    explicit Value(const Value&);
    explicit Value(Value&&) noexcept;
    Value(string_view string, bool copy=false);

    explicit Value(int32_t value);
    explicit Value(int64_t value);
    explicit Value(uint32_t value);
    explicit Value(uint64_t value);
    explicit Value(double value);
    explicit Value(bool value);

    static Value FromOpaqueValue(const AZStd::any& value);

    // Equality / comparison / swap...
    Value& operator=(const Value&);
    Value& operator=(Value&&) noexcept;

    bool operator==(const Value& rhs) const;
    bool operator!=(const Value& rhs) const;

    void Swap(Value& other) noexcept;

    // Type info...
    Type GetType() const;
    bool IsNull() const;
    bool IsFalse() const;
    bool IsTrue() const;
    bool IsBool() const;
    bool IsNode() const;
    bool IsObject() const;
    bool IsArray() const;
    bool IsOpaqueValue() const;
    bool IsNumber();
    bool IsInt() const;
    bool IsUint() const;
    bool IsDouble() const;
    bool IsString() const;

    // Object API (also used by Node)...
    Value& SetObject();
    size_t MemberCount() const;
    size_t MemberCapacity() const;
    bool ObjectEmpty() const;

    Value& operator[](KeyType name);
    const Value& operator[](KeyType name) const;
    Value& operator[](string_view name);
    const Value& operator[](string_view name) const;

    ConstMemberIterator MemberBegin() const;
    ConstMemberIterator MemberEnd() const;
    MemberIterator MemberBegin();
    MemberIterator MemberEnd();

    ConstMemberIterator FindMember(KeyType name) const;
    ConstMemberIterator FindMember(string_view name) const;
    MemberIterator FindMember(KeyType name);
    MemberIterator FindMember(string_view name);

    Value& MemberReserve(size_t newCapacity);
    bool HasMember(KeyType name);
    bool HasMember(string_view name);

    Value& AddMember(KeyType name, const Value& value);
    Value& AddMember(string_view name, const Value& value);
    Value& AddMember(KeyType name, Value&& value);
    Value& AddMember(string_view name, Value&& value);

    void RemoveAllMembers();
    void RemoveMember(KeyType name);
    void RemoveMember(string_view name);
    MemberIterator RemoveMember(MemberIterator pos);
    MemberIterator EraseMember(ConstMemberIterator pos);
    MemberIterator EraseMember(ConstMemberIterator first, ConstMemberIterator last);
    MemberIterator EraseMember(KeyType name);
    MemberIterator EraseMember(string_view name);

    // Array API (also used by Node)...
    Value& SetArray();

    size_t Size() const;
    size_t Capacity() const;
    bool Empty() const;
    void Clear();
    
    Value& operator[](size_t index);
    const Value& operator[](size_t index) const;

    ConstValueIterator Begin() const;
    ConstValueIterator End() const;
    ValueIterator Begin();
    ValueIterator End();

    Value& Reserve(size_t newCapacity);
    Value& PushBack(Value& value);
    Value& PushBack(Value&& value);
    Value& PopBack();

    ValueIterator Erase(ConstValueIterator pos);
    ValueIterator Erase(ConstValueIterator first, ConstValueIterator last);

    Array GetArray();
    const Array GetArray() const;

    // Node API (supports both object + array API, plus a dedicated NodeName)...
    bool CanConvertToNodeFromObject() const;
    Value& ConvertToNodeFromObject();
    Value& ConvertToObjectFromNode();

    void SetNode(AZ::Name name);
    void SetNode(string_view name);

    AZ::Name GetNodeName() const;
    void SetNodeName(AZ::Name name);
    void SetNodeName(string_view name);
    //! Convenience method, sets the first non-node element of a Node.
    void SetNodeValue(Value value);
    //! Convenience method, gets the first non-node element of a Node.
    Value GetNodeValue() const;

    // int API...
    int64_t GetInt() const;
    void SetInt(int64_t);

    // uint API...
    uint64_t GetUint() const;
    void SetUint(uint64_t);

    // bool API...
    bool GetBool() const;
    void SetBool(bool);

    // double API...
    double GetDouble() const;
    void SetDouble(double);

    // string API...
    string_view GetString() const;
    size_t GetStringLength() const;
    void SetString(string_view);
    void CopyFromString(string_view);

    // opaque type API...
    const AZStd::any& GetOpaqueValue()) const;
    //! This sets this Value to represent a value of an type that the DOM has
    //! no formal knowledge of. Where possible, it should be preferred to
    //! serialize an opaque type into a DOM value instead, as serializers
    //! and other systems will have no means of dealing with fully arbitrary
    //! values.
    void SetOpaqueValue(const AZStd::any&);

    // null API...
    void SetNull();

private:
    // There are a couple options for internal storage:
    // If heap allocating for all extra storage types (node/object/array/any)
    // this can be constrained to 24 bytes and use copy on write to make copy/compare
    // very cheap.

    // Alternatively we can expand to sizeof(AZStd::any) which is a whopping 128 bytes at the
    // time this was written and avoid the indirection / thraed safety issues of copy-on-write.
    // Short string optimization can be implemented with either approach.

    // If using the the copy on write model, anything stored internally as a shared_ptr will
    // detach and copy when doing a mutating operation if use_count() > 1.

    // This internal storage will not have a 1:1 mapping to the public Type, as there may be
    // multiple storage options (e.g. strings being stored as non-owning string_view or
    // owning shared_ptr<string>)
    using ValueType = AZStd::variant<
        AZStd::monostate,
        int32_t,
        int64_t,
        uint32_t,
        uint64_t,
        float,
        double,
        bool,
        string_view,
        AZStd::shared_ptr<string>,
        AZStd::shared_ptr<NodeType>,
        AZStd::shared_ptr<ObjectType>,
        AZStd::shared_ptr<ArrayType>,
        AZStd::shared_ptr<AZStd::any>
    >;
};
```

The `GenericDocument` type shall inherit from `Value` and provide an allocator instance that can be passed through as needed.

The `Path` type shall be API-identical to rapidjson, but respect the underlying value of `KeyType` (`AZ::Name`).

The Patch API will provide the ability to generate a set of patches in the JSON-patch format by doing a hierarchical comparison between two DOM values and generating a set of `GenericPatchOperation`s to apply to acquire the new state from the previous state. The initial algorithm will not be isomorphic, i.e. it will not detect objects or nodes being reparented within the hierarchy and reduce them to `Move` operations, in the interest of keeping its initial implementation straightforward and bounded to linear time.

The patch API can be used as a basis to implement the Command Pattern for undo/redo support. Patches may be generated by the `GenericPatch::GenerateDeltaPatch` API or by manually creating a sequence of `GenericPatchOperation`s.

Patch API:
```cpp
struct GenericPatchOperation
{
    enum class Type
    {
        Add,
        Remove,
        Replace,
        Copy,
        Move,
        Test
    };

    explicit GenericPathOperation();
    explicit GenericPatchOperation(Path, Type, Value);

    Path m_domPath;
    Type m_operation;
    Value m_value;

    bool Apply(Value& rootElement);
};

struct GenericPatchInfo;

struct GenericPatch
{
    AZStd::vector<GenericPatchOperation> m_operations;

    bool Apply(Value& rootElement);
    Value GetDomRepresentation() const;
    GenericPatch CreateFromDomRepresentation(Value);
};


//! Generates a set of patches such that m_forwardPatches.Apply(beforeState) shall
//! produce a document equivalent to afterState, and a subsequent m_inversePatches.Apply(beforeState)
//! shall produce the original document.
//! This patch generation strategy does a hierarchical comparison and is not
//! guaranteed to create the minimal set of patches required to transform
//! between the two states.
PatchInfo GenerateHierarchicalDeltaPatch(const Value& beforeState, const Value& afterState);

//! A set of patches for applying a change and doing the inverse operation (i.e. undoing it).
struct GenericPatchInfo
{
    GenericPatch m_forwardPatches;
    GenericPatch m_inversePatches;
};
```

`JsonBackend` shall use rapidjson's SAX interface to support efficient conversion to and from JSON without intermediate allocations. Serialization can be done recursively using a `rapidjson::Writer` or `rapidjson::PrettyWriter` while deserialization can be done with a `rapidjson::BaseReaderHandler` instance and a `rapidjson::Reader`, both of which shall forward in a straightforward manner to the visitor API.

`XmlBackend` would benefit from bringing in an XML library that supports streaming. Presently, O3DE ships with `rapidxml`, which strictly works by allocating an in-memory XML representation that needs to be translated; this worked in prototyping, but may introduce a performance burden. [Expat](https://libexpat.github.io/) is a streaming XML library written in C with an MIT license that appears to be suitable, but it has not yet been formally evaluated.

### What metrics will need to be collected for this feature?
As several systems will be built on top of the Generic DOM, safeguards shall be in place to validate performance for highlighted use cases. These shall include, but not be limited to, the following:
- Compile time validation that the size and alignment of `Value`.
- Unit tests validating individual backends and the in-memory API with high coverage.
- Integration tests validating reprentative data producing valid round-trip representations.
- Performance metrics for visitor interface performance against large data-sets.
- Performance metrics for in-memory `Value` performance performing operations on data-sets of varying sizes measured against `rapidjson`.
- Performance metrics for patch generation in data-sets ranging from 100% identical to 100% changed and various points between.

### What are the advantages of the feature?
- Consumers can share common DOM functionality such as patch generation / application.
- Consumers can serialize to any of the supported formats, and may extend the serialization layer to support any other desired DOM format, such as YAML.
- Models can be efficiently transformed and shared between disparate systems, e.g. the Document Property Editor can copy attributes from O3DE's reflection system to use in its own DOM.

### What are the disadvantages of the feature?
- The initial implementation is non-trivial, and will require test coverage to ensure feature parity with rapidjson.
- There will likely be some performance overhead compared to working directly in a given DOM format's representation, though efficient streaming through API like rapidjson's SAX should keep this minimal.

### How will this be implemented or integrated into the O3DE environment?
The implementation shall require an up-front API implementation, followed by targeted system expansion as desired.
- Initial implementation of `Value`, `GenericDocument`, and `JsonBackend` with an emphasis on unit test coverage and profiling metrics for highlighted workflows.
- Add `XmlBackend` with tests.
- Add `GenericPatch` API to support JSON patch support and delta generation.
- Replace `rapidjson` in O3DE's `JsonSerializer` with a more generic `DomSerializer. This is not needed for the initial implementation, but would long-term allow for flexibility in choosing how we choose to serialize O3DE types and help consolidate our DOM code.

### Are there any alternatives to this feature?
Monolithically aligning on using exlucisely JSON serialization is also an option. Prefabs are built heavily around `rapidjson` already, and JSON is a generally robust format that can be used to serializable arbitrary DOMs. However, this locks us into exclusively using JSON, preventing us from using XML, YAML, or other formats should we wish to use them in the future e.g. storing user settings in YAML to make hand-editing easier. This also precludes convenience features by way of the `Node` and `Any` types.

### How will users learn this feature?
This is an API-level change that is intended to be minimally disruptive. If and when O3DE's JSON serialization gets replaced by the Generic DOM API, it should just entail replacing `rapidjson::Value` usage with a corresponding `AzCore::DOM::Value`.

### Are there any open questions?
- What XML serialization library should be used? The Generic DOM benefits greatly from SAX, which `rapidxml` doesn't provide. As highlighted above, [Expat](https://libexpat.github.io/) looks like a good candidate for a drop-in XML parser with SAX support, but there may be other options.
- Is there strong desire YAML support be prioritized? While not required for any direct downstream dependencies of this work, YAML could be a helpful format for improving the ease of authoring for some systems that currently use JSON serialization, such as the Settings Registry.
- What extensions might need to be made to the patch generation API later? Currently the only in-scope patch generation is a straightforward hierarchical comparison, but as patch generation can be so crucial to performance, other algorithms and strategies may be desired.