# Asset Bundler Workflow Tests:

Testing in this area focuses on the functionality of:
* **Asset Bundler**
  * Asset Bundler Batch
  * Asset Bundler GUI
  * Asset List Comparison Operations
  * Default Seedlists
* **Missing Dependency Scanner**
* **Asset Validation Gem**
  * SeedMode
* **Bundle Mode**
* **File Tagging System**

## Common Issues to Watch For:

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
1. Console log errors/warnings/spam 
2. Asserts 
3. Improperly rendered scenes

### Common Terms:

###### Base Resources:
 * [Creating a Project Game Release Layout ](https://www.o3de.org/docs/user-guide/packaging/windows-release-builds/) <br>
 * [Asset Bundling Concepts and Terms](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/concepts/) <br>
 * [O3DE Editor](https://www.o3de.org/docs/user-guide/editor/) <br>
 * [Console](https://www.o3de.org/docs/user-guide/editor/console/) <br>
 * [Asset Cache](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-cache/) <br>

###### Asset Processor:
* [Asset Processor](https://www.o3de.org/docs/user-guide/assets/asset-processor/) <br> 
* [Missing Dependency Scanner](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/assets-resolving/) <br>

###### Asset Bundler:
 * [Asset Bundler GUI](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/overview/) <br>
 * [Asset Bundler Batch](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/asset-bundler-batch) <br>

###### Asset Types:
* [Source Assets](https://www.o3de.org/docs/user-guide/assets/pipeline/source-assets/) <br>
* [Produce Assets](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-processing/) <br>
* [Intermediate Assets](https://www.o3de.org/docs/user-guide/assets/pipeline/intermediate-assets/) <br>

###### Dependencies:
* [Source Dependencies](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#source-dependencies) <br>
* [Product Dependencies](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#product-dependencies) <br>
* [Job Dependencies](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#job-dependencies) <br>

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

|Workflow                               | Requests                                                                                                                                               | Things to Watch For                                                                                                                                                                                                                     |
|---------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Launch the Editor and load a level.   | -  Create a new level<br> -  Load an existing level                                                                                                    | -  There should be no console errors<br> -  There should be no Asset Validation warings when neither seed mode nor bundle mode are enabled                                                                                              |
| Enable Seed mode and enter game mode. | -  Have no assets in the level<br> -  Have default assets in the level<br> -  Have custom assets in the level<br> -  Add new assets                    | -  There should be no assets reported missing when no assets are loaded<br> -  Assets should be reported as missing when default or custom assets are preset<br> -  Excluded assets, such as shaders, should not be reported as missing |
| Add seedpath.                         | -  Add seedpath for level<br> -  Add seedpath for individual assets present in level<br> -  Add seedpath for assets not currently present in the level | -  Added assets and their dependencies should not report as missing<br> -  Added seedpaths not present in level should not report as missing                                                                                            |
| Remove a seedpath.                    | -  Remove seedpath for default assets<br> -  Remove seedpath for custom assets<br> -  Remove seedpath for asset not present in level                   | -  Removed seedpath of asset not present in level should not report as missing<br> -  Removed seedpath of asset present in level should report as missing                                                                               |
| Create a seedlist.                    | -  Include assets from the previous seedpath                                                                                                           |                                                                                                                                                                                                                                         |
| Add a seedlist.                       |                                                                                                                                                        | -  Added seedlist assets present in level should no longer report as missing <br> -  Added seedlist assets not present in the level should not report as missing                                                                        |
| Remove a seedlist.                    |                                                                                                                                                        | -  Removed seedlist assets present in level should report as missing <br> -  Removed seedlist of asset present in level should report as missing                                                                                        |

### Area: Bundling and Validating Assets in Bundle Mode

**Product:** Successfully create bundles from an assetlist and validate missing assets in a level. 

**Suggested Time Box:** 90 Minutes

|Workflow                                                                               | Requests                                                                                                                                               | Things to Watch For                                                                                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Generate a new seedlist and add/edit assets in the seedlist.                          | -  Saved to it's default directory<br> -  Saved to a different directory<br> -  Populated with assets from different platforms                         | -  Should successfully create seedlist<br> -  Should successfully add assets to seedlist<br> -  Should successfully save after adding/removing assets to/from existing seedlist<br> -  Should not report console errors<br> -  Should not have duplicate seedlist entries in GUI                                                                                        |
| Generate an assetlist from a seedlist file.                                           | -  Created from one or more seedlist files<br> -  Created from Default Seedlists<br> -  Created for different platforms                                | -  Should successfully create assetlist<br> -  Should successfully populate assetlist with asset dependencies<br> -  Should not report console errors<br> -  Should be able to override existing assetlists *(requires confirmation in batch)*<br> -  Should not have duplicate assetlist entries in GUI                                                                |
| Generate a rule and add steps.                                                        | -  Add a single step<br> -  Add Multiple steps                                                                                                         | -  Should successfully create rule<br> -  Should successfully update rule with saved steps<br> -  Should successfully populate information when adding a seedlist<br> -  Should not report console errors<br> -  GUI successfully updates                                                                                                                               |
| Run a rule to generate an assetlist.                                                  | -  Run on multiple platforms simultaneously<br> -  Saved to it's default directory<br> -  Saved to a different directory                               | -  Rule's operation should successfully apply to assetlist<br> -  Should successfully create assetlist<br> -  Should successfully populate assetlist with asset dependencies<br> -  Should not report console errors<br> -  Should be able to override existing assetlists *(requires confirmation in batch)*<br> -  Should not have duplicate assetlist entries in GUI |
| Generate a bundle from an assetlist.                                                  | -  Generate bundles for different platforms                                                                                                            | -  Bundle should successfully generate<br> -  Should not report console errors                                                                                                                                                                                                                                                                                          |
| Verify bundle creation in "Completed Bundles" tab.                                    |                                                                                                                                                        | -  GUI should successfully update<br> -  Should not report console errors<br> -  Bundle information should displayed correctly                                                                                                                                                                                                                                          |
| Launch Editor, create a level, and add assets.                                        | -  Use assets included in the generated bundles<br> -  Use assets not included in the generated bundles                                                |                                                                                                                                                                                                                                                                                                                                                                         |
| Enable Bundle Mode, Load Bundles, and Enter Game Mode.                                | -  Enable Bundle Mode in Log, Warning, or Error Mode<br> - Load bundles after enabling Bundle Mode<br> -  Use addseedpath and listknown asset commands | -  Should report console errors<br> -  Missing from bundle text correctly displays assets in the level missing from mounted bundles<br> -  Listknown command displays assets and dependencies from added seedpath<br> -  Missing files are displayed in the console after entering/exiting Game Mode                                                                    |
| Add missing assets to a seedlist and proceed through the flow of generating a bundle. | -  Create a new seedlist<br> -  Update an existing seedlist                                                                                            | -  Should successfully create seedlist/assetlist/bundle<br> -  Should successfully add assets to seedlist/assetlist/bundle<br> -  Should successfully save after adding assets to existing seedlist/assetlist/bundle<br> -  Should not report console errors<br> -  Should not have duplicate seedlist/assetlist/bundle entries in GUI                                  |
| Reload Bundles and Verify Missing Assets.                                             |                                                                                                                                                        | -  Should report console errors<br> -  Missing from bundle text correctly displays assets in the level missing from mounted bundles<br> -  Assets added via the new bundle should no longer reported as "missing"<br> -  Missing files are displayed in the console after entering/exiting Game Mode                                                                    |

---
