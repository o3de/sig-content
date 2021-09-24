# Scene Manifest Prefab Output Feature

# Summary:
Technical asset pipeline developers want to write out prefab products assets from a source scene asset. The pipeline developers want to use scripts to split up complex scenes from source files, to tag entities with game information, and to create complete entity-component trees with assigned assets.

The current workflow that involves exporting the scene from the DCC, using the O3DE Editor to tweak settings by removing the default mesh grouping, adding new mesh groupings, creating an entity-component tree in the Editor, assigning the meshes and materials to the entities, and finally save out the entity-component tree as a prefab asset. If the artist changes the scene again somebody will need to go through these manual steps again. The proposed system eliminates the manual workflow by allowing scripts to generate the scene to a prefab asset using the specifications by the artist.

# What is the relevance of this feature?
The main reason the teams want this is to manage the prefab levels and entity trees closer to their Digital Content Creation (DCC) packages like Maya. The scripts will be able to output the same product assets on each machine, which will ensure that scripts will out deterministic prefabs and updated prefabs from the DCC package. This feature exposes a way to automate scene prefabricated that works in the Editor so that scene development iteration can be reduced to only a few minutes (down from days). 

## Sample User Stories
- As an 3D content creator, I want the art pipeline to produce a reasonable scene prefab by default when I export a simple scene from Maya so that I can approximate a scene in the Editor.
- As a tech artist, I want to write Python scripts that prepare a variation of car prefabs from a scene of car parts so that I do not have to manual create the cars in the Editor.
- As a technical game designer, I want to script prefab product assets from a source scene asset so that we can automate content creation logic inside the Editor.
- As a lead artist, I want to script a wide variations of scene configurations using the incoming source assets so that I can review the quality of the incoming art quickly.
- As a content engineer, I want to write a script that updates all of the existing product prefab assets backed by existing source scenes so that artists are not forced to re-export all of their scenes.
- As a game artist, I want to assemble a number of cars into prefab cars from a few car bodies and car wheels so that the game designers do not have to manually assemble them in the Editor every time the cars models’ scene is updated.

# Feature design description:
The plan is to create a new scene manifest rule Prefab Group that writes out a Procedural Prefab product described inside a scene manifest. The Editor will be able to load this Procedural Prefab product asset to place into project scenes. Editor users can use these Procedural Prefab assets inside Authored Prefab templates. This new section will contain the Prefab Groups to describe procedural prefab templates to export using the scene graph from the source asset. 

The Python script API will be extended to help create these Procedural Prefabs so that custom content rules can be used to assembled Procedural Prefabs. A Python script can write out the new manifest Prefab Group sections using logic that examines the incoming source scene files (i.e. FBX) and writes out JSON that describes a Prefab template. 

The JSON template for Prefabs will be documented so that a DCC export script can write out the entire scene manifest including this new section. This would also an artist to export the scene file plus a scene manifest that includes Prefab Groups. 

The Editor will be able to use Procedural Prefab product assets the same as Editor authored prefabs. 

## Story: Making a car asset workflow

**Current Systems (before proposed solution)**

A technical game designer builds out a car scene inside a DCC tool such as SketchUp. In this scene the game designer creates a surrogate car mesh and tire mesh. The surrogate car mesh has four dummy nodes attached to the wheel locations the tire mesh instances will be attached. These nodes are named "attachment_front_right", "attachement_front_left", "attachement_back_right", and "attachement_back_left". The designer exports the scene as car_basic.fbx to the source folder. 

A game designer creates an Authored Prefab inside the Editor. The designer uses the Editor to manually create the entity tree inside the Authored Prefab:
```
Car (entity):
	mesh: carBody (asset)
	transform:
		Front Right (entity):
			mesh: tire (asset)
			transform: (moved manually to a point)
		Front Left (entity):
			mesh: tire (asset)
			transform: (moved manually to a point)
		Back Right (entity):
			mesh: tire (asset)
			transform: (moved manually to a point)
		Back Left (entity):
			mesh: tire (asset)
			transform: (moved manually to a point)
```
Later, a 3D artist creates new final art for the car body and tire meshes with new dimensions. The game designer has to manually create the car again since the old entity transform hierarchy has change so that the wheels do not fit in the original Authored Prefab for the basic car. This is true for all the cars that get final art.

**With Procedural Prefabs (after proposed solution)**

A technical game designer builds out a car scene inside a DCC tool such as SketchUp. In this scene the game designer creates a surrogate car mesh and tire mesh. The surrogate car mesh has four dummy nodes attached to the wheel locations the tire mesh instances will be attached. These nodes are named "attachment_front_right", "attachement_front_left", "attachement_back_right", and "attachement_back_left". The designer exports the scene as car_basic.fbx to the source folder. A technical script developer writes a "process_car.py" file to process the all the ```car_*.fbx``` files. The script finds all the mesh nodes (carBody, tire) and all of the attachment nodes. The script creates mesh groups for each mesh node. 

The script creates an entity tree to assemble the car:
```
Car (entity):
	mesh: carBody (asset)
	transform:
		Front Right (entity):
			mesh: tire (asset)
			transform: (data from attachment point)
		Front Left (entity):
			mesh: tire (asset)
			transform: (data from attachment point)
		Back Right (entity):
			mesh: tire (asset)
			transform: (data from attachment point)
		Back Left (entity):
			mesh: tire (asset)
			transform: (data from attachment point)
```

An Editor content designer creates a new Authored Prefab for the "Basic Car" and attaches the Procedural Prefab "car_basic.procprefab". The designer places the basic car in a scene. A 3D artist creates new final art for the car body and tire meshes with new dimensions. An Editor content designer overrides the Authored Prefab for the Basic Car with the new car body model asset and tire model asset. The 3D artist is now free to update the art at any time since the overrides will be preserved between iteration.

## Expected Workflow

. Technical content developer writes a script to process a source scene file.
. An artist creates a 3D scene using rules expected by the script and exports the source scene file.
. The script finds nodes, creates mesh groups, creates a prefab group using the node data and meshes found in the scene.
. The Editor adds an asset entry for the Procedural Prefab in the Asset Browser.
. The Editor user creates an Authored Prefab to adds a reference to the Procedural Prefab inside the Authored Prefab.

This feature will add the script API and builder rules to process Procedural Prefab product assets.

## Terminology
- Prefab – A prefabricated description of an entity-component tree in a JSON document
- Spawnable - A prefab asset that has been compiled into a spawnable prefab asset, content that can be loaded dynamically while a game is running.
- Authored Prefab - A prefab that was created by a content creator via the O3DE Editor, and is tracked as a source asset.
- Procedural Prefab - A prefab that is output from this new system, a prefab that only exists in the asset cache and is not hand authored.
- Scene File – A file that describes the 3D data such as meshes, cameras, lights, and materials organized in a node hierarchy; example file extensions FBX, DAE, STP
- Scene Manifest – a document paired with a Scene File to describe a rule set used to export product assets from a scene
- Digital Content Creation Package (DCC Package) – this is a digital asset creation package such as Blender, Maya, or 3D Studio Max
- Keystone – code name for the team and technology that collects an entity-component tree into an asset normally authored in the Editor
- Technical pipeline developers - people that are in charge of preparing assets for the O3DE engine to consume; they typically use Python scripts or even C++ code to extend the asset pipeline to transform and/or prepare custom assets

## Tenets
- We provide workflows for tech artists to work mostly in the tools they are most comfortable using to produce engine art assets.
- We allow user defined customizations to allow game teams to extend the art pipeline from DCC tools and extending the asset pipeline.
- We provide easy to follow JSON formats to enable technical pipeline developers to extend the asset pipeline from DCC tools.
- We do not enforce soft naming conventions to give customers freedom of custom developing naming conventions.

# Technical design description:

The work will be done in the PrefabBuilder gem, some work in the AzToolsFramework to update the Prefab loader, and update the Editor UX to handle using the Procedural Prefab assets in the Prefab workflows.  

## Procedural Prefab Technology Task Breakout

- Prefab Builder (gem)
-- Add abstract IPrefabGroup to describe how to create a prefab from the Scene Graph
-- Add scriptable API to wrap the Prefab system to read & write Prefab JSON
-- Create a product version of a Prefab that can be processed into spawnables
-- Add post export scene hooks to process procedural prefab product assets
-- Add an asset type for the new Procedural Prefab
- AzToolsFramework/Prefab
-- Update the framework to utilize this new procedural prefab product asset type
- Editor
-- Update the UX to handle the proper usage of a procedural prefab

### Prefab API Required

The scripters will need certain functions to use to assemble, tweak, and update Prefab JSON.

NOTE: The AZ::Interface<> is not a part of the Behavior Context, so any useful methods in one of those interfaces will need to be created using either a bus or global functions.

- Create a Prefab template
-- Input: set of entity IDs, a container entity ID, a name for the Prefab template
-- Logic: creates a template inside the Prefab system
-- Output: new template ID
- Save a Prefab template to JSON string
-- Input: template ID
-- Logic: Generate JSON string that represents a Prefab template instance
-- Output: a JSON string
		
### Entity API 

The technical content developer (i.e. technical artists or tools developers) will need API to create and update entities meant for the Editor. The team will make buses, global functions, and data models to manage editor entities.

This is the expected API to be exposed to Python scripts.

- Create an entity with default editor components
-- Input: an entity name
-- Logic: creates an entity plus all the required components to interact with the Editor
-- Output: an entity ID

- Get an editor entity's basic fields
-- Input: an entity ID and field name
-- Logic: fetches a field of the entity
-- Output: entity’s value of that field

- Update an editor entity’s basic fields such as name
-- Input: an entity ID, a field name, and a value
-- Logic: updates the entity’s field value
-- Output: result code of the operation

- Obtain an editor component from Entity
-- Input: entity ID and component type name
-- Logic: either creates or fetches the component from the editor entity
-- Output: a component ID

- Update an editor component for an entity
-- Input: entity ID and JSON document
-- Logic: uses the JSON serialization system, update the component using JSON merge patching logic
-- Output: return codes for the update logic

- Generate default JSON schema for a component type
-- Input: a type name or type ID as string values
-- Logic: searches the serialization context for a match to return a JSON of the default fields for a type with all fields included
-- Output: a JSON formed for the type with all of its fields

- Return all types with a matching pattern
-- Input: a matching string pattern of a type name
-- Logic: finds all matching types using the patterns in the serialization context
-- Output: a list of type names

### PrefabBuilder Gem Updates
The team will add data structures and build logic to Gems/Prefab/PrefabBuilder to process Procedural Prefab product assets from the scene manifest. 

#### IPrefabGroup abstract class
The abstract class to a Prefab Group that inherits from ISceneNodeGroup.

**Expected interface**

- GetPrefabDom()
-- Input: nothing
-- Logic: none
-- Output: the JSON DOM stored inside the Prefab Group 

#### PrefabGroup class
The implementation of the IPrefabGroup data structure. It will have behavior reflected so that Python script can fill out a Prefab Group.

**Key methods**

- GetPrefabDom() implementation
- GetName() override
- GetId() override
- SetPrefabDom() for testing and advanced usage
- SetName() for testing and advanced usage
- SetId() for testing and advanced usage

#### PrefabGroupBehavior

The class registered to process Prefab Group entries in the scene manifest. The behavior to prepare, update, and write out the final JSON for the Procedural Prefab product asset. The class will be a Events::AssetImportRequestBus::Handler so that it can bind with Events::PreExportEventContext() so that it can find Prefab Groups in the scene manifest. 

### Prefab Asset Handling
The team will add classes the asset type and an asset handler to load of the Procedural Prefab assets in the Editor. 

**ProceduralPrefabAssetData**
This class stores the data from a Procedural Prefab product asset. This class contains a prefab template and the JSON loaded from the product asset file. The also assigns a unique ID for Procedural Prefab assets.

**ProceduralPrefabAssetHandler**
This class loads, destroys, and creates the Procedural Prefab product asset type in the asset system. It will register the templates inside the Prefab Template system including updating the prefab templates when the assets have reloaded. The class will also expose the Editor user facing data elements such as the display name, the group name, and browser icon.

## Editor updates
The team will update the Editor UX to allow a Procedural Prefab asset into an Authored Prefab in the Entity Outliner. The Authored Prefab link to the new template instance during normal workflow so the user can add property overrides and save out the Authored Prefab as normal. The team will update the view of the Entity Outliner to indicate that the linked Prefab is a read-only Procedural Prefab. 

# What are the advantages of the feature?
The content development team can reduce scene asset iteration time from days to minutes. 

A key advantage this feature offers is a mechanism to preserve the scenes that artists are crafting in their favorite DCC package tool. An artist or designer develops mostly in a DCC tool to develop pieces of the scene and arrange those pieces of into complex scene layouts. The tech content designer works with the art team to come up with specifications that the Python script will look for to find root nodes, describe entity-node hierarchies, attach lights, assign materials and models.

The design and art teams can develop a workflow to start with a “grey world” to lay out the architecture of the scene using temporary art then slowly replace with final art using the low iteration time of this feature.

# What are the disadvantages of the feature?
A disadvantage of this feature is that requires a script developer to learn how to add scripts to the scene building pipeline. There is no default Prefab that is produced for the source scene, so a Python script will need to hook into the pipeline to explain what to make with the incoming scene graph. 

The learning curve for a Python scripter might be steep. For example, the scripts might be hard to debug since the script runs inside an Asset Builder C++ module instead of being a pure Python script.

# How will this be implemented or integrated into the O3DE environment?

This will be developed in two gems, the Editor, and possibly in the AzToolsFramework. The Prefab gem has a PrefabBuilder which will have most of the new code, the PythonAssetBuilder gem’s script library will include helper Python script to assist with assembling the Prefab Groups, and the Editor UX will be updated to use a Procedural Prefab asset in the Asset Browser, Entity Outliner, and Entity Inspector.

# Are there any alternatives to this feature?
**Automate Prefabs inside the Editor**

It is possible to operate the Editor using Python scripts using the provide behavior from the Prefab technology. This solution updates the Prefab framework to provide behavior, data structures, and components to Python so that a scripter can assemble a Prefab entity tree from a source scene asset. An artist or designer would execute a script inside the editor to update a Prefab template.

The reason this is not the proposed solution is that it is too far away from DCC. The content tech developer’s highest request is to author data as close to the DCC tool they normally use as possible, so the proposed solution is more aligned to the original customer request.  Further, the Entity and Prefab API will still need to be done to complete this solution. Since those API will be developed during this feature work, this solution to be ready to be solved.

**Full GUI Authored Prefab Editor**
The team create an extension to the Scene Manifest GUI in the Editor to create Prefab groups like Mesh groups. The team coordinates with the UX team to design, develop, and code a UX to extend the Scene Manifest editor via the Asset Browser’s “Edit Settings…” menu for source scene files. This UX is used to add Mesh Groups and other rules to the scene manifest present.

The reason this is not the proposed solution is that it is too far away from DCC. This solution requires an artists or designer to load up the source scene in the Editor and create a Prefab Group manually in the “Edit Settings…” form. While this is not a destructive process, it can not be automatically updated if the scene specifications (like size of car tires) have changes.

# How will users learn this feature?
Early notice of this feature will be announced in Discord on the GitHub Discussions page. The documentation will be added to the Prefab workflow to explain to content teams how to automate Prefab asset creation using Python scripts. This feature works side by side with the existing Prefab authoring workflow so new or existing users will be able to benefit after learning the workflow. 

# Are there any open questions?
**Can a source scene file output more than one Procedural Prefab product assets?**

The plan is to have the Prefab Group Behavior class generate any number of Procedural Prefabs product assets found in the Scene Manifest.

**Can a Procedural Prefab be created from outside the Asset Process + Scene Builder?**

Yes.

The definition of a Prefab Group can be added to a scene manifest file from an external source. For example, a DCC tool that is exporting the foo.fbx file can also write out a foo.fbx.assetinfo scene manifest with a Prefab Group.

**Can an Editor user assign property values to Procedural Prefab instances?**

Yes. Sort of.

The workflow is to create an Authored Prefab then drag-drop a Procedural Prefab template into the Authored Prefab. From there the Editor user can add overrides to property values as normal.

**Can an Editor user alter a Procedural Prefab template?**

No.

The Procedural Prefab can only be changed by a change to the source scene manifest or the Python script that assembles the Prefab Group. The concept is to enforce the Prefab Group as the source of authority for the Procedural Prefab product asset.

**How will the O3DE data structures be updated?**

It is possible to update any data structure that has been registered with the Serialization Context (SC) with the JSON Serializer. The plan is to use a “best fit” system using JSON to fill out the fields for SC classes. If a team updates components and/or other data structures that are in Procedural Prefabs they will need to update the Prefab Groups to process the existing Procedural Prefab product assets.
