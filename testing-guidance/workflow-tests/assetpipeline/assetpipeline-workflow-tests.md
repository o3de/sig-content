# Asset Pipeline Workflow Tests:

Testing in this area focuses on the functionality of the Asset Processor, Asset Bundler, Scene Processing, and Asset Validation.

## Common Issues to Watch For:

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
1. Console log errors/warnings/spam 
2. Asserts 
3. Improperly rendered scenes

### Common Terms:

**Base Resources:** <br>
 [Asset Pipeline O3DE Resources](https://www.o3de.org/docs/user-guide/assets/pipeline/) <br>
 [Asset Pipeline Concepts and Terms](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/concepts/) <br>

**In-depth Resources:** <br>
 [Asset Processor](https://www.o3de.org/docs/user-guide/assets/asset-processor/) <br> 
 [Asset Cache](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-cache/) <br>
 [Asset Bundler](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/)
 [O3DE Editor](https://www.o3de.org/docs/user-guide/editor/) <br>
 [Source Assets](https://www.o3de.org/docs/user-guide/assets/pipeline/source-assets/) <br> 
 [Product Assets and Asset Processing](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-processing/) <br>
 [Job Logs](https://www.o3de.org/docs/user-guide/assets/asset-processor/debugging/#view-asset-processor-logs) <br>
 [Scan Directories](https://www.o3de.org/docs/user-guide/assets/pipeline/scan-directories/) <br>
 [Console](https://www.o3de.org/docs/user-guide/editor/console/) <br>
 [Source Dependencies](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#source-and-job-dependencies) <br>
 [Product Dependencies](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#product-dependencies) <br>
 
 

---

## Feature: Asset Processor

**Project Requirements:** Any project with available source files can be used.

**Resources:** <br>
 [Asset Processor Interface](https://www.o3de.org/docs/user-guide/assets/asset-processor/interface/) <br>
 [Asset Processor Configuration](https://www.o3de.org/docs/user-guide/assets/asset-processor/configuration/)

### Area: Asset Ingestion | Job and Product Validation

**Product:** Successfully processed assets are in cache and visible in the Asset Browser.

**Suggested Time Box:** 20 Minutes

| Workflow                                                                | Requests                                                                                                                   | Things to Watch For                                                                                                                                                                                          |
|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Launch Asset Processor.                                                 | Source assets already exist within a project scan directory.                                                                  | -   Excessive Job Analysis Scanning<br> -   Excessive Processing time for files<br> -   Asset Processor Errors<br> -   Asset Processor successfully processing source files into the cache as product files  |
| Add new assets to the project.                                          | Create and place new assets within a project scan directory:<br> -  Supported source assets<br> -   Unsupported Source assets | -   Asset Processor successfully processing supported source assets and generating product assets in the cache<br> -   AP does not process unsupported source assets                                         |
| Open new asset Job Log from Jobs tab right-click context menu.          |	Opens the log file up in filesystem default program for .log files.                                                        | -   Asserts on opening the log file<br> -   Job log file has unnecessary/unhelpful log spam<br> -   Job log file is empty on a job with warnings, errors, or failures                                        |
| Open new asset in Asset Browser from Jobs tab right-click context menu. | Open up Asset Browser<br> *Note: Editor must already be open.*                                                             | Processed assets can be discovered and interacted with in the Asset Browser.                                                                                                                                 |

### Area: Source and Product Asset GUI Navigation

**Product:**  Source Asset and Product Asset are discoverable.

**Suggested Time Box:** 10 Minutes

|Workflow                                                                           | Requests                                                                                                         | Things to Watch For                                                                                                                                                                                                                                                       |
|-----------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| From Jobs tab jump to source asset from right-click context menu.                 | Source Assets tab is opened to the the selected source asset.                                                    | Source asset isn't found.                                                                                                                                                                                                                                                 |
| Jump to a product asset from selected asset's right-click context menu.           | Product Assets tab is opened to the the selected source asset.                                                   | Product asset isn't found.                                                                                                                                                                                                                                                |
| Jump back to Source Asset from selected asset's info panel "Source Asset" button. | Source Assets tab is opened to the the selected source asset.                                                    | Source asset isn't found.                                                                                                                                                                                                                                                 |
| Reprocess asset from selected Source Asset's right-click context menu.            | -   Asset processor performs job analysis on file<br> -   Asset is reprocessed<br> -   Asset is is product cache | -   Excessive Job Analysis Scanning<br> -   Excessive processing time for files<br> -   Asset processor Errors<br> -   Asset Processor successfully reprocesses assets and stores product assets them in the Asset Cache<br> -   New Job Log and Product files are output |
| Open file in Explorer from selected asset's right-click context menu.             | File Explorer is opened to the source file's location.                                                           | -   Wrong folder is opened<br> -   Folder fails to open                                                                                                                                                                                                                   |

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

|Workflow                                                            | Requests                                                                                                                                                                               | Things to Watch For                                                                                                                                                                                         |
|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Launch the Editor.                                                 | Editor and Asset Processor open.<br> Asset Processor processes scene files without failing.                                                                                            | -   Excessive Job Analysis Scanning<br> -   Excessive Processing time for files<br> -   Asset Processor Errors<br> -   Asset Processor successfully processing source files into the cache as product files |                                                                                                                                    |
| Open Asset Browser from processed scene asset in Asset Processor.  | Asset Browser panel opens within Editor.                                                                                                                                               | - Console errors                                                                                                                                                                                            |
| Search for scene assets and open the scene settings tool.          | -   Right-click context menu<br> -   Double click<br> -   Use various scene assets with different combinations of meshes, actors, PhysX, and other supported scene settings properties | -   Console errors<br> -   Scene Settings properties load up<br> EG: Meshes, Actors, PhysX<br> -   File Progress Processing report                                                                          |
| Change Settings Properties.                                        | -   Modify mesh properties<br> -   Modify actor properties<br> -   Modify PhysX properties                                                                                             | -   Console Errors -   Asset Processing errors                                                                                                                                                              |
| Create a new level and add a scene to the level.                   | -   Drag & drop into editor viewport from Asset Browser<br> -   Adding to an entity component                                                                                          | -   Scene loads in<br> -   Scene renders correctly                                                                                                                                                          |
| Enter Game Mode.                                                   |                                                                                                                                                                                        | -   Scene loads in<br> -   Scene renders correctly<br> -   Any scene behaviors render correctly                                                                                                             |

### Area: Loading and Viewing Characters, Motions, and Morph Targets

**Product:** Successfully processed scene assets can be added to a level and rendered.

**Suggested Time Box:** 60 Minutes

|Workflow                               | Requests                                                                                                          | Things to Watch For                                                                                      |
|---------------------------------------|-------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Open a character in Animation Editor. | -   Can be done without an open level<br> -   Scene has a skinned mesh, actor, motion, and morph target animation | -   Console errors<br> -   Character fails to render                                                     |
| Open up Animation layout.             | Layout > Animation                                                                                                | -   Console errors                                                                                       |
| Load and Play a motion.               | - Can also load multiple motions with shift clicking.                                                             | -   Console errors<br> -   Motion fails to load<br> -   Model render breaks<br> -   Motion fails to play |
| Open up the Character layout.         | Layout > Character                                                                                                | -   Console errors                                                                                       |
| Select the the morph targets.         | -   Single Morph<br> -   Multiple morphs                                                                          | -   Model render breaks                                                                                  |
| Adjust the morph targets.             | Move the sliders for the selected morph targets.                                                                  | -   Morph is rendered correctly<br> -   Model render breaks                                              |

---

## Feature: Asset Bundler GUI and Asset Validation

**Project Requirements:** Any project with Asset Validation gem and multiple platforms enabled.

**Resources**: <br>
 [Validating Seedpaths and Seedlists](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/verifying-bundles/asset-validation-gem/) <br>
 [Validating Bundled Assets](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/verifying-bundles/bundle-mode/)

**Note:** You will need to enter game mode for Seedmode and BundleMode to report missing assets.

### Area: Validating Seedpaths and Seedlists in Seed Mode

**Product:** Seedlists with all assets ready for packaging.

**Suggested Time Box:** 30 Minutes

|Workflow                               | Requests                                                                                                                                                  | Things to Watch For                                                                                                                                                                             |
|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Launch the Editor and load a level.   | -   Create a new level<br> -   Load an existing level                                                                                                     | -   Console errors<br> -   Asset Validation warings when neither seed mode nor bundle mode are enabled                                                                                          |
| Enable Seed mode and enter game mode. | -   Have no assets in the level<br> -   Have default assets in the level<br> -   Have custom assets in the level<br> -   Add new assets                   | -   Assets reported missing when no assets are loaded<br> -   Assets not reported as missing when default or custom assets are preset<br> -   Excluded assets should not be reported as missing |
| Add seedpath.                         | -   Add seedpath for level<br> -   Add seedpath for individual assets present in level<br> -   Add seedpath for assets not currently present in the level | -   Added assets and their dependencies are still reported missing<br> -   Added seedpaths not present in level are reported missing                                                            |
| Remove a seedpath.                    | -   Remove seedpath for default assets<br> -   Remove seedpath for custom assets<br> -   Remove seedpath for asset not present in level                   | -   Removed seedpath of asset not present in level is not reported<br> -   Removed seedpath of asset present in level reports a missing asset                                                   |
| Create a seedlist.                    | -   Include assets from the previous seedpath                                                                                                             |                                                                                                                                                                                                 |
| Add a seedlist.                       |                                                                                                                                                           | -   Seedlist assets present in level no longer reported as missing <br> -   Seedlist assets not present in the level are not reported                                                           |
| Remove a seedlist.                    |                                                                                                                                                           | - Seedlist assets present in level are once again reported as missing                                                                                                                           |

### Area: Bundling and Validating Assets in Bundle Mode

**Product:** Successfully create bundles from an assetlist and validate missing assets in a level. 

**Suggested Time Box:** 90 Minutes

|Workflow                                                                               | Requests                                                                                                                                                 | Things to Watch For |
|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|
| Generate a new seedlist and add/edit assets in the seedlist.                          | -   Saved to it's default directory<br> -   Saved to a different directory<br> -   Populated with assets from different platforms                        | -   Fails to create seedlist<br> -   Fails to add assets to seedlist<br> -   Fails to save after adding/removing assets to/from existing seedlist<br> -   Console errors<br> -   Duplicate seedlist entries in GUI                                                                    |
| Generate an assetlist from a seedlist file.                                           | -   Created of one or more seedlist files<br> -   Created from Default Seedlists<br> -   Created for different platforms                                 | -   Fails to create assetlist<br> -   Fails to populate assetlist with asset dependencies<br> -   Console errors<br> -   Unable to override existing assetlists<br> -   Duplicate assetlist entries in GUI                                                                            |
| Generate a rule and add steps.                                                        | -   Add Multiple steps | -   Fails to create rule<br> -   Fails to update rule with saved steps<br> -   Steps fail to populate information when adding a seedlist<br> -   Console errors<br> -   GUI fails to update                                                                                                                                                                                                                             |
| Run a rule to generate an assetlist.                                                  | -   Run on multiple platforms simultaneously<br> -   Saved to it's default directory<br> -   Saved to a different directory                              | -   Rule's operation is not applied to assetlist<br> -   Fails to create assetlist<br> -   Fails to populate assetlist with asset dependencies<br> -   Console errors<br> -   Unable to override existing assetlists<br> -   Duplicate assetlist entries in GUI                       |
| Generate a bundle from an assetlist.                                                  | -   Generate bundles for different platforms                                                                                                             | -   Bundle fails to generate<br> -   Console errors                                                                                                                                                                                                                                   |
| Verify bundle creation in "Completed Bundles" tab.                                    |                                                                                                                                                          | -   GUI fails to update<br> -   Console Errors<br> -   Bundle information is displayed correctly                                                                                                                                                                                      |
| Launch Editor, create a level, and add assets.                                        | -   Use assets included in the generated bundles<br> -   Use assets not included in the generated bundles                                                |                                                                                                                                                                                                                                                                                       |
| Enable Bundle Mode, Load Bundles, and Enter Game Mode.                                | -   Enable Bundle Mode in Log, Warning, or Error Mode<br> - Load bundles after enabling Bundle Mode<br> -   Use addseedpath and listknown asset commands | -   Console errors<br> -   Missing from bundle text correctly displays assets in the level missing from mounted bundles<br> -   Listknown command displays assets and dependencies in from added seedpath<br> -   Missing files are displayed in the console after entering Game Mode |
| Add missing assets to a seedlist and proceed through the flow of generating a bundle. | -   Create a new seedlist<br> -   Update an existing seedlist                                                                                            | -   Fails to create seedlist<br> -   Fails to add assets to seedlist<br> -   Fails to save after adding assets to existing seedlist<br> -   Console errors<br> -   Duplicate seedlist entries in GUI                                                                                  |
| Reload Bundles and Verify Missing Assets.                                             |                                                                                                                                                          | -   Console errors<br> -   Missing from bundle text correctly displays assets in the level missing from mounted bundles<br> -   Assets added via the new bundle are no longer reported as "missing"<br> -   Missing files are displayed in the console after entering Game Mode       |

---
