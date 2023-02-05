# Scene Pipeline Workflow Tests:

Testing in this area focuses on the functionality of:
* **Scene Processing Pipeline**
  * Scene API
    * Scene Graph
    * Scene Builder
    * User Defined Properties
  * Scene Settings Files (Scene Manifest)
  * Scene Settings Tool



## Common Issues to Watch For:

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
1. Console log errors/warnings/spam 
2. Asserts 
3. Improperly rendered or assembled scenes 
4. HotReloading issues

### Common Terms:

###### Base Resources:
* [Asset Processor](https://www.o3de.org/docs/user-guide/assets/asset-processor/) <br> 
* [Asset Cache](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-cache/) <br>
* [O3DE Editor](https://www.o3de.org/docs/user-guide/editor/) <br>
  * [Console](https://www.o3de.org/docs/user-guide/editor/console/) <br>

###### Scene Processing Pipeline:
* [Scene API](https://www.o3de.org/docs/user-guide/assets/scene-pipeline/scene-api/) <br>
  * [Scene Graph](https://www.o3de.org/docs/user-guide/assets/scene-pipeline/scene-graph/) <br>
  * [Scene Builder](https://www.o3de.org/docs/user-guide/assets/scene-pipeline/scene-builder/) <br>
  * [User Defined Properties](https://www.o3de.org/docs/user-guide/assets/scene-pipeline/scene-api-udp/) <br>
* [Source Scene Assets](https://www.o3de.org/docs/user-guide/assets/scene-settings/) <br>
  * [Scene Format Support](https://www.o3de.org/docs/user-guide/assets/scene-settings/scene-format-support/) <br>
* [Scene Settings File / Scene Manifest](https://www.o3de.org/docs/user-guide/assets/scene-pipeline/scene-manifest/) <br>
* [Scene Settings Tool](https://www.o3de.org/docs/user-guide/assets/scene-settings/scene-settings/) <br>

---

## Feature: Scene Settings Tool

**Project Requirements:** Source scene assets with meshes, actors, physx, and other supported settings properties.

**Resources:** <br>
 [Scene Settings](https://www.o3de.org/docs/user-guide/assets/scene-settings/) <br>
 [3D Scene Format Support](https://www.o3de.org/docs/user-guide/assets/scene-settings/scene-format-support/) <br>
 [Scene Settings Interface](https://www.o3de.org/docs/user-guide/assets/scene-settings/interface/)

### Area: Process, Modify, and Load Scene Assets

**Product:** Successfully processed scene assets can be added to a level and rendered.

**Suggested Time Box:** 90 Minutes

|Workflow                                                            | Requests                                                                                                                                                                            | Things to Watch For                                                                                                                                                                                                                                                          |
|--------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Launch the Editor.                                                 | Editor and Asset Processor open.<br> Asset Processor processes scene files without failing.                                                                                         | -  Should not experience excessive job analysis scanning<br> -  Should not experience excessive processing time for files<br> -  Should not encounter asset processor errors<br> -  Asset Processor should successfully process source files into the cache as product files |                                                                                                                                    |
| Open Asset Browser from processed scene asset in Asset Processor.  | Asset Browser panel opens within Editor.                                                                                                                                            | -  Should not report any console errors                                                                                                                                                                                                                                      |
| Search for scene assets and open the scene settings tool.          | -  Right-click context menu<br> -  Double click<br> -  Use various scene assets with different combinations of meshes, actors, PhysX, and other supported scene settings properties | -  Should not report any console errors<br> -  Scene Settings properties should load correctly<br> -  File progress processing report should display correctly                                                                                                               |
| Change Settings Properties.                                        | -  Modify mesh properties<br> -  Modify actor properties<br> -  Modify PhysX properties                                                                                             | -  Should not report any console errors -  Should not encounter any asset processing errors                                                                                                                                                                                  |
| Create a new level and add a scene to the level.                   | -  Drag & drop into editor viewport from Asset Browser<br> -  Adding to an entity component                                                                                         | -  Scene should load in correctly<br> -  Scene should render correctly                                                                                                                                                                                                       |
| Enter Game Mode.                                                   |                                                                                                                                                                                     | -  Scene should load in correctly<br> -  Scene should render correctly<br> -  Any scene behaviors should render correctly                                                                                                                                                    |

### Area: Loading and Viewing Characters, Motions, and Morph Targets

**Product:** Successfully processed scene assets can be added to a level and rendered.

**Suggested Time Box:** 60 Minutes

|Workflow                               | Requests                                                                                                        | Things to Watch For                                                                                                                                |
|---------------------------------------|-----------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Open a character in Animation Editor. | -  Can be done without an open level<br> -  Scene has a skinned mesh, actor, motion, and morph target animation | -  Should not report any console errors<br> -  Character should renders correctly                                                                  |
| Open up Animation layout.             | Layout > Animation                                                                                              | -  Should not report any console errors                                                                                                            |
| Load and Play a motion.               | - Can also load multiple motions with shift clicking.                                                           | -  Should not report any console errors<br> -  Motion should load correct<br> -  Model should render correctly<br> -  Motion should play correctly |
| Open up the Character layout.         | Layout > Character                                                                                              | -  Should not report any console errors                                                                                                            |
| Select the the morph targets.         | -  Single Morph<br> -  Multiple morphs                                                                          | -  Model should render correctly                                                                                                                   |
| Adjust the morph targets.             | Move the sliders for the selected morph targets.                                                                | -  Morph should render correctly<br> -  Model should render correctly                                                                              |

---
