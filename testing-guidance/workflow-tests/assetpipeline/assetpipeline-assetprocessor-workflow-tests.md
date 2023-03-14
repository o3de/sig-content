# Asset Processor Workflow Tests:

Testing in this area focuses on the functionality of:
* **Asset Processor**
  * Asset Processor Batch
  * Asset Processor GUI
* **Job Analysis**
  * XML Schema System
  * Copy Jobs
  * Process Jobs
  * Dependencies
    * Source Dependencies
    * Product Dependencies
    * Job Dependencies
* **Source Control**
  * Perforce Integration
* **Asset Cache Server**
* **Asset Relocation Tool**
* **Missing Dependency Scanner**
* **Intermediate Assets**
* **Virtual File System**

## Common Issues to Watch For:

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
1. Console log errors/warnings/spam 
2. Asserts 
3. Improperly rendered scenes

### Common Terms:

###### Base Resources:
* [Asset Pipeline O3DE Resources](https://www.o3de.org/docs/user-guide/assets/pipeline/) <br>
* [Asset Pipeline Concepts and Terms](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/concepts/) <br>
* [O3DE Editor](https://www.o3de.org/docs/user-guide/editor/) <br>
* [Console](https://www.o3de.org/docs/user-guide/editor/console/) <br>
* [Asset Processor](https://www.o3de.org/docs/user-guide/assets/asset-processor/) <br> 
* [Asset Cache](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-cache/) <br>
 
###### Asset Processor:
* [Asset Processor Batch](https://www.o3de.org/docs/user-guide/assets/asset-processor/asset-processor-batch/) <br>
* [Asset Processor GUI](https://www.o3de.org/docs/user-guide/assets/asset-processor/interface/) <br>
* [Asset Cache Server](https://www.o3de.org/docs/user-guide/assets/asset-processor/asset-cache-server/) <br>
* [Asset Relocation Tool](https://www.o3de.org/docs/user-guide/assets/asset-processor/move-assets/) <br>
* [Missing Dependency Scanner](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/assets-resolving/) <br>
* [Virtual File System](https://www.o3de.org/docs/user-guide/) <br>

###### Job Analysis:
* [Copy Jobs](https://www.o3de.org/docs/user-guide/assets/asset-types/#copy-jobs) <br>
* [Process Jobs](https://www.o3de.org/docs/user-guide/assets/asset-types/) <br>
* [XML Schema System](https://www.o3de.org/docs/user-guide/) <br>
* [Job Logs](https://www.o3de.org/docs/user-guide/assets/asset-processor/debugging/#view-asset-processor-logs) <br>

###### Asset Types:
* [Source Assets](https://www.o3de.org/docs/user-guide/assets/pipeline/source-assets/) <br>
* [Produce Assets](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-processing/) <br>
* [Intermediate Assets](https://www.o3de.org/docs/user-guide/assets/pipeline/intermediate-assets/) <br>

###### Dependencies:
* [Source Dependencies](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#source-dependencies) <br>
* [Product Dependencies](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#product-dependencies) <br>
* [Job Dependencies](https://www.o3de.org/docs/user-guide/assets/pipeline/asset-dependencies-and-identifiers/#job-dependencies) <br>
 
###### Source Control:
* [Perforce Integration](https://www.o3de.org/docs/user-guide/) <br>

###### Asset Processor Configuration:
* [Scan Directories](https://www.o3de.org/docs/user-guide/assets/pipeline/scan-directories/) <br>
* [File Tagging System](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/file-tagging/) <br>

---

## Feature: Asset Processor

**Description:** Testing in this area will focus on Asset Processor and Asset Processor Batch usage.

**Project Requirements:** Any project with available source files can be used.

**Resources:** <br>
 [Asset Processor Interface](https://www.o3de.org/docs/user-guide/assets/asset-processor/interface/) <br>
 [Asset Processor Configuration](https://www.o3de.org/docs/user-guide/assets/asset-processor/configuration/)

### Area: Asset Ingestion | Job and Product Validation

**Product:** Successfully processed assets are in cache and visible in the Asset Browser.

**Suggested Time Box:** 20 Minutes

| Workflow | Requests  | Things to Watch For |
|----------|-----------|---------------------|
| **Launch Asset Processor** | <ol><li>Source assets already exist within a project scan directory.</li></ol> | <ul><li>There should be no excessive job analysis scanning.</li><li>There should be no excessive processing time for files.</li><li>Asset Processor errors should be descriptive and actionable.</li><li>Asset Processor should successfully process source files into the cache as product files.</li></ul> |
| **Add new assets to the project** | <ol><li>Create and place new assets within a project scan directory: <ul><li>Supported source assets.</li><li>Unsupported Source assets.</li></ul></li></ol> | <ul><li>Asset Processor should successfully process supported source assets and generate product assets in the cache.</li><li>AP should not process unsupported source assets.</li></ul> |
| **Open new asset Job Log from Jobs tab right-click context menu** | <ol><li>Opens the log file up in filesystem default program for .log files.</li></ol> | <ul><li>Should have no asserts when opening the log file.</li><li>Job log file should not have unnecessary/unhelpful log spam.</li><li>Job log file should not be empty on a job with warnings or errors.</li></ul> |
| **Open new asset in Asset Browser from Jobs tab right-click context menu** | <ol><li>Open up Asset Browser.</ol> | <ul><li>Processed assets should be discoverable and interactable within the Asset Browser.</li></ul> |

### Area: Source and Product Asset GUI Navigation

**Product:**  Source Asset and Product Asset are discoverable.

**Suggested Time Box:** 10 Minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **From Jobs tab jump to source asset from right-click context menu** | <ol><li>Source Assets tab is opened to the the selected source asset.</li></ol> | <ul><li>Source asset should be found within the tab.</li></ul> |
| **Jump to a product asset from selected asset's right-click context menu** | <ol><li>Product Assets tab is opened to the the selected source asset.</li></ol> | <ul><li>Product asset should be found within the tab.</li></ul> |
| **Jump back to Source Asset from selected asset's info panel "Source Asset" button** | <ol><li>Source Assets tab is opened to the the selected source asset.</li></ol> | <ul><li>Source asset should be found within the tab.</li></ul> |
| **Reprocess asset from selected Source Asset's right-click context menu** | <ol><li>Asset processor performs job analysis on file.</li><li>Asset is reprocessed.</li><li>Asset is product cache.</li></ol> | <ul><li>There should be no excessive job analysis scanning.</li><li>There should be no excessive processing time for files.</li><li>Asset Processor errors should be descriptive and actionable.</li><li>Asset Processor should successfully process source files into the cache as product files.</li><li>New Job Logs and Product files should be output.</li></ul> |
| **Open file in Explorer from selected asset's right-click context menu** | <ol><li>File Explorer is opened to the source file's location.</li></ol> | <ul><li>Wrong folder is opened.</li><li>Folder fails to open.</li></ul> |

### Area: Asset Processor Batch basic actions

**Project Requirements**
* AssetProcessorBatch present in the build folder. If it is not it can be built using `cmake --build build/<build folder name> --target AssetProcessorBatch --config profile` command from the root engine folder.

**Product:**  Files modified via Asset Processor Batch.

**Suggested Time Box:** 10 Minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Prepare Command Line to execute Asset Processor Batch commands** | <ol><li>Open Command line in the engine build folder (for example `C:\o3de-install\bin\Windows\profile\Default\`) and confirm AssetProcessorBatch can be executed for a chosen project (for example `AssetProcessorBatch.exe --project-path=C:\Users\<user>\O3DE\Projects\<project>`).</li></ol> | <ul><li>Executing AssetProcessorBatch for a chosen project processes assets without any asserts.</li></ul> |
| **Delete an asset** | <ol><li>Delete an existing asset via Asset Processor Batch (for example `AssetProcessorBatch.exe --project-path=C:\Users\<user>\O3DE\Projects\<project> --delete=C:\Users\<user>\O3DE\Projects\<project>\test.txt --confirm`).</li></ol> | <ul><li>Deleted asset should be removed from disk.</li></ul> |
| **Rename an asset** | <ol><li>Rename an existing asset via Asset Processor Batch (for example `AssetProcessorBatch.exe --project-path=C:\Users\<user>\O3DE\Projects\<project> --move=C:\Users\<user>\O3DE\Projects\<project>\test.txt,C:\Users\<user>\O3DE\Projects\<project>\test1.txt --updateReferences --confirm`).</li></ol> | <ul><li>Asset's name should be changed (and updated in Editor if `--updateReferences` argument is used).</li></ul> |
| **Move an asset** | <ol><li>Move an existing asset via Asset Processor Batch (for example `AssetProcessorBatch.exe --project-path=C:\Users\<user>\O3DE\Projects\<project> --move=C:\Users\<user>\O3DE\Projects\<project>\test1.txt,C:\Users\<user>\O3DE\Projects\<project>\Assets\test1.txt --updateReferences --confirm`).</li></ol> | <ul><li>Asset should be moved to the chosen path (and updated in Editor if `--updateReferences` argument is used).</li></ul> |

---

## Feature: Job Analysis

**Description:** Testing in this area will focus on XMLSchema as well as Source, Product and Job Dependencies scanning.

### Area: XMLSchema Dependencies Scan

**Project Requirements:**
*  XMLSchema asset with dependent files setup. For example the existing `Font.xmlschema` along with `Engine/Fonts/` (on Pre-built SDK engine their paths are `o3de-install/Assets/Engine/Schema/Font.xmlschema` and `o3de-install/Assets/Fonts/` respectively) assets could be used.

**Platforms:**
* Windows
* Linux

**Product:** An XMLSchema file scanned for missing dependencies.

**Suggested Time Box:** 20 Minutes
| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Perform scan for missing dependencies with none missing** | <ol><li>In the *Asset Processor* navigate to the *Assets* > *Product Assets* tab and find your `.xmlschema` file (for example `pc/schema/font.xmlschema`).</li><li>In the *Missing Product Dependencies* section to the right, click the *Scan file* button.</li><li>In the CLI navigate to your engine build location (for example `C:\o3de-install\bin\Windows\profile\Default\`) and run the AssetProcessorBatch command to scan for the missing dependencies for the fonts (for example `AssetProcessorBatch --project-path=C:\Users\<user>\O3DE\Projects\<project> --zeroAnalysisMode --dsp=%fonts%.xml`).</li></ol> | <ul><li>The `.xmlschema` asset can be found in the *Asset Processor*.</li><li>*Missing Product Dependencies* log returns `No missing dependencies found` after clicking the *Scan file* button.</li><li>There are no `Missing dependency:` lines under each `Scanning for missing dependencies:` line in the CLI.</li></ul> |
| **Perform scan for missing dependencies with some missing** | <ol><li>Move your `.xmlschema` file out of it's original path (for example `o3de-install/Assets/Engine/Schema/Font.xmlschema`).</li><li>In the *Asset Processor* navigate to the *Assets* > *Product Assets* tab and find an `.xml` asset that was dependent on the removed `.xmlschema` asset (for example `pc/font/vera-italic.xml`).</li><li>In the *Missing Product Dependencies* section to the right, click the *Scan file* button.</li><li>In the CLI navigate to your engine build location (for example `C:\o3de-install\bin\Windows\profile\Default\`) and run the AssetProcessorBatch command to scan for the missing dependencies for the fonts (for example `AssetProcessorBatch --project-path=C:\Users\<user>\O3DE\Projects\<project> --zeroAnalysisMode --dsp=%fonts%.xml`).</li><li>Remember to return the `.xmlschema` file back to it's original path for any further use.</li></ol> | <ul><li>An `.xml` asset can be found in the *Asset Processor*.</li><li>*Missing Product Dependencies* log returns the name of missing dependency after clicking the *Scan file* button (`Vera-italic.ttf` in the case of `vera-italic.xml`).</li><li>There are `Missing dependency:` lines under each `Scanning for missing dependencies:` line in the CLI.</li></ul> |

### Area: Source, Product and Job Dependencies

**Project Requirements:**
* Any project with at least one asset in each *Asset Processor* > *Assets* category: *Source Assets*, *Intermediate Assets* and *Product Assets* with one or more of *Incoming Dependencies* (*Dependencies - In*) or *Outgoing Dependencies* (*Dependencies - Out*).

**Platforms:**
* Windows
* Linux

**Product:** Scanned project cache and assets with dependencies verified to be displayed in the *Asset Processor*.

**Suggested Time Box:** 20 Minutes
| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Perform an Asset Processor Scan** | <ol><li>In the *Asset Processor* navigate to the *Assets* > *Settings* tab and in the *Full Scan* section click the *Start Scan* button.</li></ol> | <ul><li>Assets are fully scanned with no notable errors occuring.</li></ul> |
| **Verify the Job Dependencies** | <ol><li>In the *Asset Processor* navigate to the *Assets* > *Source Assets* tab and verify that assets display Dependencies in the pane to the right (for example `engine.json`).</li><li>In the *Asset Processor* navigate to the *Assets* > *Intermediate Assets* tab and verify that assets display Dependencies in the pane to the right (for example `materials/reflectionprobe/reflectionprobevisualization_generated.materialtype`).</li><li>In the *Asset Processor* navigate to the *Assets* > *Product Assets* tab and verify that assets display Dependencies in the pane to the right (for example `pc/fonts/vera-italic.font`).</li></ol> | <ul><li>*Source Assets* contain assets that display Dependnecies (either *In* or *Out* or both).</li><li>*Intermediate Assets* contain assets that display Dependnecies (either *In* or *Out* or both).</li><li>*Product Assets* contain assets that display Dependnecies (either *In* or *Out* or both).</li></ul> |

---

## Feature: AP Source Control Integration

**Description:** Testing in this Area will focus on connecting and working with Perforce (p4) as a source control tool.

**Project Requirements:**

* Perforce installed on the machine.
* O3DE added to the p4 depot (without cache folders).

**Platforms:**

* Windows

**Product:**  Assets that are shared on p4 server.

**Suggested Time Box:** 60 Minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Enable Source Control plugin** | <ol><li>Open the O3DE Editor.</li><li>Close the Welcome to O3DE window or load any level.<li>Click LMB on the Perforce plugin icon.</li><li>Enable the connection.</li></ol> | <ul><li>Perforce plugin is present in the bottom bar of the O3DE Editor.</li><li>Clicking on the p4 plugin shows Status switch and Settings.</li><li>Settings are grayed out when p4 connection is set to disabled.</li><li>Perforce plugin is green while enabled, and gray while disabled.</li><li>Enabling and disabling p4 connection prints appropriate message in the Editor's console.</li></ul> |
| **Configure connection settings** | <ol><li>Click LMB on the Perforce plugin icon.</li><li>Enable the p4 connection.</li><li>Open Settings in the Perforce plugin.</li><li>Provide correct values for each setting:<ul><li>Server, User and Workspace should be filled with information from p4v. Those are the same information that user needs to log into p4 applications.</li><li>Charset should be set to "none".</li></ul><li>Press OK.</li></ol> | <ul><li>User is able to povide the information necessary for p4 connection.</li><li>Filing Settings with invalid information results in p4 plugin color change to orange and message in Editor's console about invalid Perforce configuration.</li><li>User is able to disconnect from Perforce changing the Work Online switch.</li></ul> |
| **Add asset to depot** | <ol><li>Import any asset to the project you are working on.</li><li>Find in Asset Browser newly imported asset.</li><li>Click RMB on it.</li><li>From the context menu select "Add to Source Control".</li><li>Open p4v application and log to it</li><li>Submit changes.</li></ol> | <ul><li>Imported asset shows in Asset Browser.</li><li>When Perforce connection is enabled, source control options are present in the context menu.</li><li>Adding asset to source control prints appropriate messages in the Editor's console.</li><li>Asset is marked as "Add" in the perfoce application and changes can be submitted.</li><li>Asset Processor process asset without issues.</li></ol> |
| **Check Out asset** | <ol><li>In the Asset Browser, find any asset that is already added to the source control.</li><li>Click RMB on it.</li><li>From the context menu select "Check Out".</li><li>Open p4v application and log into it</li><li>Submit changes.</li></ol> | <ul><li>When Perforce connection is enabled, source control options are present in the context menu.</li><li>Checking Out assets prints appropriate messages in the Editor's console.</li><li>Asset is marked as "Checked Out" in the perfoce application and changes can be submitted.</li><li>Asset Processor process asset without issues.</li></ol> |
| **Reverting changes** | <ol><li>Chceck Out an asset.</li><li>Verify that asset is Checked Out in the Perforce application and change can be submitted.</li>In Asset Browser inside the O3DE, click RMB on the asset.</li><li>From the context menu select "Undo Check Out".</li><li>In the Perforce application, verify that Chceked Out asset dissapeard from changelist.</li></ol> | <ul><li>When Perforce connection is enabled, source control options are present in the context menu.</li><li>Checking Out assets prints appropriate messages in the Editor's console.</li><li>Asset is marked as "Checked Out" in the perfoce application.</li><li>Check Out assets can be reverted.</li><li>Reverting Check Out prints appropriate messages in the Editor's console.</li><li>After reverting Check Out, changes are removed from p4v changelist.</li><li>Asset Processor process asset without issues.</li></ol> |
| **Getting last version** | <ol><li>In the Asset Browser find an asset that was changed by other user.</li><li>Click RMB on it.</li><li>From the context menu select "Get Latest Version".</li></ol> | <ul><li>When Perforce connection is enabled, source control options are present in the context menu.</li><li>Getting latest version of asset from source control prints appropriate messages in the Editor's console.</li><li>Changes to the asset are visible without need of restarting the Editor.</li><li>Editor remains stable.</li><li>Asset Processor process asset without issues.</li></ol> |

---

## Feature: Asset Cache Server

**Description:** Testing in this area should focus on the Asset Cache Server and Client side.

### General Docs
* [Asset Cache Server](https://www.o3de.org/docs/user-guide/assets/asset-processor/asset-cache-server/)

### Common Issues to Watch For

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
- Server side generating Cache
- Path to the Cache location
- Ability of the client to connect to the server side
- Ability of Client side to retrieve file from the server side

## Workflows

### Area: Create the transfer directory 

**Project Requirements**
* None

**Editor Platforms:**
* Windows
* Linux

**Docs:**
* [Asset Cache Server](https://www.o3de.org/docs/user-guide/assets/asset-processor/asset-cache-server/)

**Product:** a shared directory that all the team members can access over the network.

**Suggested Time Box:** 10 minutes per platform.

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Creating a shared folder that will store Cache and be accessible over the network** | <ol><li> Create a folder.</li><li>Share the folder over the network.</li></ol> | <ul><li>Asset Processor has permission to write to the folder.</li><li>Folder is accessible over the network.</li></ul> |

### Area: Using the Shared Cache Tab

**Project Requirements**
* None

**Editor Platforms:**
* Windows
* Linux

**Product:** Working Asset Cache Server (Shared Cache)

**Suggested Time Box:** 15 minutes per platform.

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Set shared cache mode** | <ol><li>Change Shared Cache Mode.</li></ol> | <ul><li>Default mode in inactive.</li><li>Server - The Asset Processor will archive product assets on a remote folder.</li><li>Client - The Asset Processor will attempt to retrieve archive product assets from a remote folder.</li></ul> |
| **Select the transfer directory via *Select a remote folder*** | <ol><li>Input path to the server cache directory.</li></ol> | <ul><li>Path is saved.</li><li>When the “Save Changes” button is clicked, the system will detect the validity of the transfer directory.</li></ul> |
| **Manage shared cache patterns** | <ol><li>Create a new pattern.</li></ol> | <ul><li>Pattern can be created.</li><li>Pattern can be named.<li>Pattern type can be changed.</li><li>Pattern attribute can be changed.</li><li>Pattern can be removed using the Trash icon.</ul> |
| **Save Changes** | <ol><li>Save the made changes.</li></ol> | <ul><li>The Save Changes and Discard buttons become enabled when a change to the settings has been detected.</li><li>The Save Changes commits the changes to a settings file. The Discard button reverts the panel’s values.</li><li>Changes are saved if Asset Processor is reopened.</li><li>The settings file is written to the `{project_folder}/Registry/asset_cache_server_settings.setreg` file.</li><li>If the Remote Folder is set to an invalid folder location, then an error dialog will show up and no settings will be written.</li></ul> |

### Area: Running Asset Processor in Server Mode

**Project Requirements**
* None

**Editor Platforms:**
* Windows
* Linux

**Docs:**
* [Asset Cache Server](https://www.o3de.org/docs/user-guide/assets/asset-processor/asset-cache-server/)
* [RegEx](https://en.wikipedia.org/wiki/Regular_expression)

**Game Launcher Supported Platforms:**
* Windows
* Linux

**Product:** Asset Processor archiving product assets to a remote folder

**Suggested Time Box:** 30 minutes per platform.

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Set shared cache mode to Server** | <ol><li>Change Shared Cache Mode to Server.</li></ol> | <ul><li>Change is saved.</li></ul> |
| **Select the transfer directory via *Select a remote folder*** | <ol><li>Input path to the server cache directory.</li></ol> | <ul><li>Path is saved.</li><li>Path is detected as valid by the system.</li></ul> |
| **Add a Wildcard Pattern** | <ol><li>Add a new Pattern.</li><li>Name the pattern.</li><li>Set Pattern Type to Wildcard.</li><li>Set Pattern to "*.fbx" or any other file type.</li></ol> | <ul><li>Pattern is saved.</li><li>Pattern's name and Pattern is valid.</li><li>Pattern is Enabled.</li></ul> |
| **Add a RegEx Pattern** | <ol><li>Add a new Pattern.</li><li>Name the pattern.</li><li>Set Pattern Type to RegEx.</li><li>Set Pattern to a RegEx expression.</li></ol> | <ul><li>Pattern is saved.</li><li>Pattern's name and RegEx expression is valid.</li><li>Pattern is Enabled.</li></ul> |
| **Save Changes** | <ol><li>Save the made changes.</li></ol> | <ul><li>Save button can be pressed.</li><li>System detects validity of the transfer directory.</li><li>Asset Processor starts archiving product assets to a remote folder according to the Patterns.</li></ul> |

### Area: Running Asset Processor in Client Mode

**Project Requirements**
* None

**Editor Platforms:**
* Windows
* Linux

**Docs:**
* [Asset Cache Server](https://www.o3de.org/docs/user-guide/assets/asset-processor/asset-cache-server/)
* [RegEx](https://en.wikipedia.org/wiki/Regular_expression)

**Product:** Asset Processor retrieving archive product assets from a remote folder.

**Suggested Time Box:** 30 minutes per platform.

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Set shared cache mode to Client** | <ol><li>Change Shared Cache Mode to Client.</li></ol> | <ul><li>Change is saved.</li></ul> |
| **Select the transfer directory via *Select a remote folder*** | <ol><li>Input path to the server cache directory.</li></ol> | <ul><li>Path is saved.</li><li>Path is detected as valid by the system.</li></ul> |
| **Add a Wildcard Pattern** | <ol><li>Add a new Pattern.</li><li>Name the pattern.</li><li>Set Pattern Type to Wildcard.</li><li>Set Pattern to **.fbx* or any other file type.</li></ol> | <ul><li>Pattern is saved.</li><li>Pattern's name and Pattern is valid.</li><li>Pattern is Enabled.</li></ul> |
| **Add a RegEx Pattern** | <ol><li>Add a new Pattern.</li><li>Name the pattern.</li><li>Set Pattern Type to RegEx.</li><li>Set Pattern to a RegEx expression.</li></ol> | <ul><li>Pattern is saved.</li><li>Pattern's name and RegEx expression is valid.</li><li>Pattern is Enabled.</li></ul> |
| **Save Changes** | <ol><li>Save the made changes</li></ol> | <ul><li>Save button can be pressed.</li><li>System detects validity of the transfer directory.</li><li>Asset Processor starts retrieving archive product assets from a remote folder.</li></ul> |

---

## Feature: Missing Dependency Scanner

**Description:** Testing in this area should focus on finding presence of missing dependencies.

### General Docs
* [Resolving Missing Assets](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/assets-resolving/)

### Common Issues to Watch For

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
- Dependency scanner not working
- Dependency scanner not finding missing dependencies

### Workflows

### Area: Running Dependency Scanner

**Project Requirements**
* AssetProcessorBatch present in the build folder. If it is not it can be built using `cmake --build build/<build folder name> --target AssetProcessorBatch --config profile` command from the root engine folder.
* [testReferenceFile.xml](asset-processor-attachments/testReferenceFile.xml) downloaded.

**Editor Platforms:**
* Windows
* Linux

**Product:** Finding presence of missing dependencies

**Suggested Time Box:** 15 minutes per platform.

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Add test file** | <ol><li>Place *testReferenceFile.xml* to Asset folder of the Project folder.</li><li>Launch Asset Processor for the project.</li><li>Wait for all Assets to be processed.</li><li>Shut down Asset Processor.</li></ol> | <ul><li>Asset errors.</li><li>Runtime errors of Asset Processor.</li></ul> |
| **Run Missing Dependency Scanner** | <ol><li>Run `AssetProcessorBatch --project-path = %project-path% --zeroAnalysisMode` command.</li></ol> | <ul><li>Missing Dependency Scanner startup errors.</li><li>Missing Dependency Scanner runntime errors.</li><li>`-dsp` flag is functional.</li><li>`-dsp` flag is filtering Assets as expected.</li><li>Dependency Scanner shows missing dependencies.</li></ul> |
| **Delete Asset with missing Dependency** | <ol><li>Remove the `testReferenceFile.xml` file from Assets.</li><li>Re-run Asset processor.</li><li>Shut down Asset Processor.</li><li>Run Dependency Scanner again.</li></ol> | <ul><li>Dependency Scanner does not output errors concerning the deleted file.</li></ul> |

### Area: Finding a false positive in the missing dependency scanner

**Project Requirements**
* AssetProcessorBatch present in the build folder. If it is not it can be built using `cmake --build build/<build folder name> --target AssetProcessorBatch --config profile` command from the root engine folder.

**Editor Platforms:**
* Windows
* Linux

**Product:** Verified a presence of false possitive missing dependency.

**Suggested Time Box:** 15 minutes per platform.

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Identify a false positive missing dependenct asset** | <ol><li>Go to *Tools* > *Asset Editor* in the Editor.</li><li>Open the `exclude.filetag` asset.</li><li>Verify that it contains a *File Tag Map* with `editoronly` *File Tag* included (for example `*.slice`)</li><li>Open *Asset Processor* and navigate to *Assets* > *Product Assets*.</li><li>Find a `*.slice` asset (for example `pc/engineassets/slices/defaultlevelsetup.slice`) using search bar and select it.</li><li>In the right pane click the *Scan file* button.</li></ol> | <ul><li>*Asset Editor* can be opened.</li><li>`exclude.filetag` asset can be opened.</li><li>EditorOnly tag asset can be located.</li><li>`File matches EditorOnly or Shader tag, ignoring for missing dependencies search` is printed to the log under the *Scan file* button.</li></ul> |

---

## Feature: Virtual File System

Testing in this area should focus on the functionality of running the Android application in assets by Virtual File System (VFS) packaging mode.

### Common Issues to Watch For

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
- Be aware of network connection errors from the mobile device to Asset Processor running on PC
- Rendering artifacts when running the deployed level on the Android devices
- Asserts and errors that appear in the Android log while running the deployed level
- Immediate crashes when launching the app on the Android device
- Crashes that occur after some time leaving the app running

### Editor Platforms:

- Windows

### Game Launcher Supported Platforms:

- Android

### Documents and Common Terms

* [O3DE User Guide: Android Support for O3DE](https://www.o3de.org/docs/user-guide/platforms/android/)

### Area: Build Android in VFS packaging mode

### Project Requirements
* Create a new default project.
* Create `<project-path>/Registry/AssetProcessor.setreg` file to generate android assets in the cache:
````
{
    "Amazon": {
        "AssetProcessor": {
            "Settings": {
                "Platforms": {
                    "android": "enabled"
                }
            }
        }
    }
}
````
* Make sure `<project-path>/Platform/Android/android_project.json` exists and that it has android settings section in it. Example:
````
"android_settings": {
    "package_name": "org.o3de.DefaultProject",
    "version_number": 1,
    "version_name": "1.0.0.0",
    "orientation": "landscape"
},
````
* Build the default project.
* Open the Editor and create a default level.
* Create `<project-path>/Registry/autoexec.game.setreg` file to set the starting level:
````
{
    "O3DE": {
        "Autoexec": {
            "ConsoleCommands": {
                "LoadLevel": "DefaultLevel"
            }
        }
    }
}
````
* Create `<project-path>/Registry/bootstrap.setreg` file to enable remote filesystem on Android:
````
{
    "Amazon": {
        "AzCore": {
            "Bootstrap": {
                "android_remote_filesystem": 1,
                "android_connect_to_remote": 1,
                "android_wait_for_connect": 1
            }
        }
    }
}
````
* Run Asset Processor and wait until all assets have been processed.

**Product:** Android application running in VFS packaging mode.

**Suggested Time Box:** 3 hours for requirements + 1 hour for the workflow.

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Assets by VFS (minimum assets in sdcard)** | <ol><li>Uninstall the default project's app from device (if previously installed).</li><li>Follow the steps from https://www.o3de.org/docs/user-guide/platforms/android/generating_android_project_windows/ to build and deploy with the following alterations:<ol type="a"><li>Use the default project created instead of `o3de-atom-sampleviewer`.</li><li>Use `o3de-build/android_vfs` for `%O3DE_BUILD_ROOT%` instead of `o3de-build`.</li><li>When running `generate_android_project.py` command use `VFS` for `%O3DE_ANDROID_ASSET_MODE%` and remove `--include-apk-assets`.</li><li>When running `deploy_android.py` command use `BOTH` for `%O3DE_ANDROID_DEPLOY_TYPE%`.</li></ol><li>With the mobile device connected to PC run this command from the console: `adb reverse tcp:45643 tcp:45643`. <p>**IMPORTANT**: This command has to be run every time the adb connection is lost or killed, which is each time the cable is plugged, but also it can happen often without knowing. If when running the app it shows refuse-connections messages in the log, that's a signal that adb reverse command needs to be run again before running the app.</p></li></ol> | <ul><li>Binaries will be inside the android APK package. Unzip `O3DE_BUILD_ROOT/app/build/outputs/apk/profile/app-profile.apk` and verify `lib/arm64-v8a` folder contains `.so`.</li><li>Minimum assets necessary to launch the app will be inside the device's sdcard (not in the APK package). Go to `/sdcard/Android/data/org.o3de.DefaultProject/files` to check they are there. Minimum assets are:<ol type="a"><li>`engine.json` file.</li><li>Bootstrap `.setreg` files.</li><li>Config files inside `config` folder.</li></ol></li><li>All the assets will be obtained via network connecting to the PC and read from `<project-path>/Cache/android/`.</li><li>After running the app, the execution's log and user folder will be generated in PC (`<project-path>/user/`).</li></ul> |

### Area: Run default level on Android device

### Project Requirements
* You've built Android in Assets by VFS packaging mode.

**Game Launcher Supported Platforms:**
* Android

**Product:** A default level is launched correctly on Android devices in Assets by VFS packaging mode.

**Suggested Time Box:** 10 minutes.

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| **Run default level** | <ol><li>Follow the build steps from the previous area in this document.</li><li>Launch the default project's app in the android device.</li><li>Use Android Studio (or any other method) to check the log output from the default project's app. Select the `O3DE_BUILD_ROOT` folder when opening Android Studio to open the project and after all the processing is done the Logcat tool window will be available and once the app is running on the device you can filter the output by app.| <ul><li>The level opens successfully.</li><li>It doesn't crash after a while (for example a couple of minutes).</li><li> The level is rendered correctly without artifacts.</li><li>Framerate is 15fps or above. This depends on the device used, but the key thing to look for is that there are no spamming errors or warnings in the log indicating that something wrong is happening every frame.</li></ul> |

---