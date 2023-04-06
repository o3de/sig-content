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

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Launch the Editor** | <ol><li>Editor and Asset Processor open.</li><li>Asset Processor processes scene files without failing.</li></ol> | <ul><li>Should not experience excessive job analysis scanning.</li><li>Should not experience excessive processing time for files.</li><li> Should not encounter asset processor errors.</li><li>Asset Processor should successfully process source files into the cache as product files.</li></ul> |
| **Open Asset Browser from processed scene asset in Asset Processor** | <ol><li>Asset Browser panel opens within Editor.</li></ol> | <ul><li>Should not report any console errors.</li></ul> |
| **Search for scene assets and open the scene settings tool** | <ol><li>Right-click context menu.</li><li>Double click.</li><li>Use various scene assets with different combinations of meshes, actors, PhysX, and other supported scene settings properties.</li></ol> | <ul><li>Should not report any console errors.</li><li>Scene Settings properties should load correctly.</li><li>File progress processing report should display correctly.</li></ul> |
| **Change Settings Properties** | <ol><li>Modify mesh properties.</li><li>Modify actor properties.</li><li>Modify PhysX properties.</li></ol> | <ul><li>Should not report any console errors.</li><li>Should not encounter any asset processing errors.</li></ul> |
| **Create a new level and add a scene to the level** | <ol><li>Drag & drop into editor viewport from Asset Browser.</li><li>Adding to an entity component.</li></ol> | <ul><li>Scene should load in correctly.</li><li>Scene should render correctly.</li></ul> |
| **Enter Game Mode** | <ol><li>Enter Game Mode in Editor (for example *Ctrl + G*).</li></ol> | <ul><li>Scene should load in correctly.</li><li>Scene should render correctly.</li><li>Any scene behaviors should render correctly.</li></ul> |

### Area: Loading and Viewing Characters, Motions, and Morph Targets

**Product:** Successfully processed scene assets can be added to a level and rendered.

**Suggested Time Box:** 60 Minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Open a character in Animation Editor** | <ol><li>Can be done without an open level.</li><li>Scene has a skinned mesh, actor, motion, and morph target animation</li></ol> | <ul><li>Should not report any console errors.</li><li>Character should renders correctly.</li></ul> |
| **Open up Animation layout** | <ol><li>Go to *Layout* > *Animation*.</li></ol> | <ul><li>Should not report any console errors.</li></ul> |
| **Load and Play a motion** | <ol><li>Can also load multiple motions with shift clicking.</li></ol> | <ul>Should not report any console errors.</li><li>Motion should load correct.</li><li>Model should render correctly.</li><li>Motion should play correctly.</li></ul> |
| **Open up the Character layout** | <ol><li>Go to *Layout* > *Character*.</li></ol> | <ul><li>Should not report any console errors.</li></ul>  |
| **Select the the morph targets** | <ol><li>Single Morph.</li><li>Multiple morphs.</li></ol> | <ul><li>Model should render correctly.</li></ul> |
| **Adjust the morph targets** | <ol><li>Move the sliders for the selected morph targets.</li></ol> | <ul><li>Morph should render correctly.</li><li>Model should render correctly.</li></ul> |

---

### Area: Scene Manifest

**Description:** Testing in this Area will focus on .fbx files, its manifest and processing them trough Asset Processor..

**Project Requirements:**

* An .fbx file that is not added to the project and has no settings file (.fbxassetinfo file).
* Python Asset Builder gem enabled in the project.

**Platforms:**

* Windows
* Linux

**Product:**  Assets that can be used in the Editor and be configured via its manifest.

**Suggested Time Box:** 60 Minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Using assets that do not have scene manifest** | <ol><li>Import an .fbx file to the project.</li><li>Find imported asset in the Asset Browser.</li><li>Drag it into the Viewport.</li><li>In the Asset Browser, click RMB on the imported fbx and select *Edit Scene Settings* from dropdown menu.</li></ol> | <ul><li>Asset is imported to the project.</li><li>User is able to create entity that uses the imported asset.</li><li>Asset can be opened in Scene Settings.</li><li>Scene Settings shows correct information.</li><li>Asset Processor process .fbx file without issues and does not fail any job while working with the .fbx file.</li><li>Opening the asset in Scene Settings does not create its .fbxassetinfo file.</li><li>O3DE uses default settings written in the DCC tool used to create the asset.</li></ul> |
| **Creating scene manifest** | <ol><li>In the Asset Browser, find an .fbx file that does not have scene manifest.</li><li>Click RMB on it and select *Edit Scene Settings* from dropdown menu.</li><li>Edit the fbx, e.g. add modifier.</li><li>Save changes.</li></ol> | <ul><li>Asset can be opened in Scene Settings.</li><li>Asset can be modified in Scene Settings.</li><li>Saving modifications does not fail in Asset Processor and Scene Settings.</li><li>Scene manifest (`.fbxassetinfo` file) is created after modifying the fbx file in Scene Settings.</li></ul> |
| **Changing scene manifest in Scene Settings** | <ol><li>Open in Scene Settings an fbx that have its own manifest.</li><li>Add a new Mesh group.</li><li>Change the name of the created mesh group.</li><li>Save changes.</li><li>Create a new mesh entity in the Viewport.</li><li>As a mesh assign a newly created mesh model.</li></ol> | <ul><li>Asset can be opened in Scene Settings.</li><li>Asset can be modified in Scene Settings.</li><li>Saving modifications does not fail in Asset Processor and Scene Settings.</li><li>Changes made in Scene Settings are applied in asset and can be used in the scene.</li></ol> |
| **Changing scene manifest manually** | <ol><li>Open in Scene Settings an fbx that have its own manifest.</li><li>Add Comment modifier.</li><li>Write any comment in the comment box, e.g. "Test123".</li><li>Save Changes and close the Scene Settings.</li><li>Find the `.fbxassetinfo` file under project's root.</li><li>Open the file in text editor.</li><li>Find comment you created.<ul><li>Easy way to find it is via Search function.</li></ul></li><li>Change the comment, e.g. "Test123" to "Test124".</li><li>Open the fbx in the Scene Settings again.</li></ol> | <ul><li>Asset can be opened in Scene Settings.</li><li>Asset can be modified in Scene Settings.</li><li>Saving modifications does not fail in Asset Processor and Scene Settings.</li><li>Changing the scene manifest results in Asset Processor reprocessing the asset.</li></ul> |
| **Deleting scene manifest** | <ol><li>Open in Scene Settings an fbx that have its own manifest.</li><li>Add a new Mesh group.</li><li>Change the name of the created mesh group.</li><li>Save changes and close the Scene Settings.</li><li>Verify that new mesh model was created</li><li>Find the `.fbxassetinfo` file under project's root.</li><li>Delete it.</li><li>Open the fbx in the Scene Settings again.</li></ol> | <ul><li>Asset can be opened in Scene Settings.</li><li>Asset can be modified in Scene Settings.</li><li>Saving modifications does not fail in Asset Processor and Scene Settings.</li><li>Deleting the manifest results in Asset Processor reprocessing the asset.</li><li>After deleting the scene manifest, asset uses the default settings written in the DCC tool used to create the asset.</li></ul> |
| **Using python scripts** | <ol><li>In the Asset Browser find the `geom_group.fbx`.</li><li>Open it in Scene Settings.</li><li>Go to *File* > *Reset* setting to default.</li><li>Make chnges to the fbx, e.g. create a comment using Comment modifier.</li><li>Save changes.</li>Go to *File* > *Assign script*.</li><li>Select python script to overrite scene manifest.<ul><li>You can find scripts under `Project\Gem\PythonTests\PythonAssetBuilder`.</li></ul></li></ol> | <ul><li>Assets and files provided in gems are available in the project.</li><li>Asset can be opened in Scene Settings.</li><li>Settings of the fbx can be resetted to default.</li><li>Asset can be modified in Scene Settings.</li><li>Python script can be assign to the fbx.</li><li>Settings of the fbx are overwritten by python script.</li><li>Modifying the fbx results in Asset Processor and Scene Settings to reprocess the file successfully.</li><li>Editor remains stable.</li></ul> |

---

### Area: User Defined Properties

**Project Requirements:**
* An asset to be used with a User Defined Property (UDP). For example a material created in the Material Editor and saved to the `<project>/Assets/` folder. This can be a simple `Standard PBR` material with `Base Color` > `Color` changed to `0,255,0` for easy recognition. 
* An .fbx asset with a UDP prepared in the DCC tool. For example in Blender this could be a Cube with no material assigned, `Object Properties` > `Custom Properties` set to `o3de_default_material` with value `assets/materialname.azmaterial` (if the material's path is `<project>/Assets/MaterialName.material`) and exported with the `Include` > `Custom Properties` enabled.

**Platforms:**
* Windows
* Linux

**Product:** An asset imported from a DCC tool utilizing a material from the project specified in the UDP.

**Suggested Time Box:** 20 Minutes
| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Verify that the Editor utilizes UDPs** | <ol><li>Verify that the custom material mentioned in the **Project Requirements** section is processed by the *Asset Processor*.</li><li>Verify that the custom .fbx asset with UDP mentioned in the **Project Requirements** section is processed by the *Asset Processor*.</li><li>Find the custom .fbx asset in the *Asset Browser*, then drag and drop it into the Viewport.</li></ol> | <ul><li>Custom material can be created in the *Material Editor* and is properly processed by the *Asset Processor*.</li><li>Custom .fbx is properly processed by the *Asset Processor*.</li><li>Custom .fbx uses the custom material after being dragged and dropped into the *Viewport* with no further adjustments.</li></ul> |

---

### Area: Soft Naming

**Project Requirements:**
* An .fbx asset with meshes utilizing soft naming prepared in the DCC tool. For example in Blender this could be two Cube meshes side by side named `Cube_lod0` and `Cube_lod1` respectively in the `Scene Collection` > `Collection`.

**Platforms:**
* Windows
* Linux

**Product:**  An asset imported from a DCC tool automatically adjusting LODs in the Editor due to mesh soft naming in that DCC tool.

**Suggested Time Box:** 20 Minutes
| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Verify that the custom asset is processed** | <ol><li>Verify that the custom .fbx asset with _lod mesh soft name suffixes mentioned in the **Project Requirements** section is processed by the *Asset Processor*.</li></ol> | <ul><li>Custom .fbx is properly processed by the *Asset Processor*.</li></ul> |
| **Verify that the soft name suffixes are utilized in the Editor** | <ol><li>Create an entity and add a *Mesh* component to it.</li><li>Assign the custom .fbx to the *Mesh* component's *Model Asset* property.</li><li>With the *Mesh* entity in view, move the camera close to it and move the camera away from it.</li></ol> | <ul><li>An entity with *Mesh* component can be created.</li><li>Custom .fbx can be assigned to the *Model Asset* property.</li><li>If the custom .fbx is setup as described in the **Project Requirements** section, only one cube out of two should be visible simultaneously - one when the camera is close to the *Mesh* entity and one when the camera is far from the *Mesh* entity.</li></ul> |

---