### Summary
This feature proposes adding Python bindings for the widgets and components contained within the AzQtComponents library through the use of the Shiboken Python module.

### What is the relevance of this feature
Currently, Python bindings are available for standard Qt components and widgets, through the use of the PySide2 library. This allows users to modify the editor, or create external tools, but they are not able to access the expanded toolset contained within the AzQtComponents library; these can only be used from C++ unless bindings are added to create a Python interface.


### Feature design description
This proposal introduces an additional build stage to the project. Components to be bound are described in an xml typesystem file that defines the relationship between the C++ and Python types. Normally, only the class needs to be declared all methods and member variables should be handled automatically, but changes can be made, by modification of function definitions, or code injection to ensure correct behavior in cases where Python would otherwise be unable to handle them.
The typesystem file is then used by the Shiboken generator to produce an intermediate C++ wrapper file, which is in turn used to build a library which can be imported into Python.

In most cases, developer interaction will be limited to the use of the Python module itself, in order to interact with, or extend the editor or external tools. Changes to the typesystem file will only be necessary if new items are added to, or removed from AzQtComponents, or changes are made that require changes in order to function, for example, the addition of a method that requires additional code in order that Python can interface with it correctly. An example of this would be a C++ method which stores data in a variable passed in as a pointer. As Python would be unable to handle this, it would make sense to modify the return values to pass the value back, making the return value a tuple if necessary, i.e. there already is a return value.

### Technical design description

The build process is handled using CMake. A CMakeLists.txt file will be created controlling the two stages necessary to produce the library. Essentially this consists of
```Cpp

add_custom_command(OUTPUT ${list_generated_sources}
                   COMMAND ${shiboken_generator_exe_path}
                   ${shiboken_options} ${wrapped_header} ${typesystem_file}
                   DEPENDS ${wrapped_header} ${typesystem_file}
                   INCLUDE_DIRECTORIES PRIVATE ${CMAKE_CURRENT_LIST_DIR}..
                   IMPLICIT_DEPENDS CXX ${wrapped_header}
                   WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
                   COMMENT Running generator for ${typesystem_file}.)
                   
add_library(${bindings_library} SHARED ${list_generated_sources})
                    
```
Where

`${bindings_library}` is the name chosen for the library. This is the module name that is imported into Python.

`${list_generated_sources}` is a list of all the cpp wrapper files that are produced by the shiboken run. There is one for the Python module itself, one for the AzQtComponents library, then one for each reflected class. Names are all lower-case, and are prepended with the name of their containing namespace as appropriate.
```Cpp
set(list_generated_sources
    ${bindings_library}_module_wrapper.cpp
    azqtcomponents_wrapper.cpp   
    azqtcomponents_extendedlabel_wrapper.cpp
```

`${typesystem_file}` is the xml file that describes the types, methods and classes to be reflected in the library. This is done as follows
```xml
typesystem package=modulename
    namespace-type name=AzQtComponents generate=yes
        object-type name=ExtendedLabel
    namespace-type
typesystem
```
This will bind the `ExtendedLabel` component and all its methods.

Some classes will require additional code. Some examples of this are as follows

1. Classes that contain enums
```xml
    object-type name=AutoCustomWindowDecorations
        enum-type name=Mode
    object-type
```

2. Classes that contain sub classes or struct declarations.
```xml
    object-type name=FancyDocking
        object-type name=WidgetGrab
    object-type
```
3. Methods that pass values back in pointer arguments.
As Python cannot handle this, the approach taken is to remove the argument in question from the method, and return the value instead, making the return value a tuple if necessary.
```xml
    object-type name=FileDialog           
        modify-function signature=GetSaveFileName(QWidget,QString,QString,QString,QString,QFlags&lt;QFileDialogOption&gt;) 
            modify-argument index=5
                remove-argument
                remove-default-expression 
            modify-argument
            modify-argument index=return
                replace-type modified-type=(QString, QString selectedFilter)
            modify-argument
            inject-code class=target position=beginning
                bool selectedFilter_ = nullptr;
                %RETURN_TYPE retval_ = %CPPSELF.%FUNCTION_NAME(%ARGUMENT_NAMES, &amp;selectedFilter_);
            inject-code
        modify-function
    object-type
```

`${wrapped_header}` is an include file alongside the typesystem file, to register the files that contain the declarations of the items to be bound
```Cpp
#pragma once

#include AzQtComponentsComponentsExtendedLabel.h 
```



It will be necessary to upgrade the PySide 3rdParty library we are currently using we will need to move to version 5.15.2-rev1 in order to correctly support the current version of Qt. 



### What are the advantages of the feature
This feature will allow users to modify or create editor extensions, or standalone tools, through the use of the Python language, which may be faster for them. It gives them the flexibility to develop using the langauge of their choice.
The advantage of using Shiboken, over other methods, is that as the PySide2 library for Qt is created using Shiboken, it is fairly comprehensive, and already has a library of typesystem files covering Qt data types and classes. This will greatly speed up the development process due to the removal of the overhead caused by having to bind the Qt types.

### What are the disadvantages of the feature
The main disadvantage of using Shiboken to generate the bindings is that it cannot cope with all scenarios. It cannot, for example, bind template classes. Currently, there is only one widget that this affects
```Cpp
template typename WidgetType
    class LogicalTabOrderingWidget  public WidgetType
```
which cannot be bound. 

There will be a small increase to build time if this feature is added to the editor build system rather than being built as a standalone process.

### How will this be implemented or integrated into the O3DE environment
This work will involve changes to the CMake build files.

### Are there any alternatives to this feature
Two other possible approaches were considered
 PyBind - This would be far more labor intensive, as all items to be exposed has to be specified in additional code. This method would also necessitate bindings being written for any Qt types that are used, as these don't already exist as is the case with Shiboken.
 BehaviorContext - This would work in the same way that the creation of the azlmbr Python library occurs in the current code. Additional reflection code would have to be added to all classes to be exposed, with all required methods specified. The biggest limitation with this method is that the library creation happens at editor startup, making it unsuitable for use outside the editor. 

### How will users learn this feature
Experienced Python users will have no trouble making use of this feature. Documentation is desirable to explain differences in method signatures where changes have been made between the C++ and Python code. There will also be a blog post for Technical Artists explaining how to use Python to access the AzQtComponents classes.

### Are there any open questions
As the O3DE project is large and complex, it's entirely possible that during the course of the work, situations will be encountered that Shiboken can't handle. Work will have to be carried to to discover whether the issue can be worked around, or whether an item has to be omitted from the reflection.
