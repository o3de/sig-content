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

**Project Requirements:** Any project with available source files can be used.

**Resources:** <br>
 [Asset Processor Interface](https://www.o3de.org/docs/user-guide/assets/asset-processor/interface/) <br>
 [Asset Processor Configuration](https://www.o3de.org/docs/user-guide/assets/asset-processor/configuration/)

### Area: Asset Ingestion | Job and Product Validation

**Product:** Successfully processed assets are in cache and visible in the Asset Browser.

**Suggested Time Box:** 20 Minutes

| Workflow                                                                | Requests                                                                                                                      | Things to Watch For                                                                                                                                                                                                                                                                      |
|-------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Launch Asset Processor.                                                 | Source assets already exist within a project scan directory.                                                                  | -  There should be no excessive job analysis scanning<br> -  There should be no excessive processing time for files<br> -  Asset Processor errors should be descriptive and actionable <br> -  Asset Processor should successfully process source files into the cache as product files  |
| Add new assets to the project.                                          | Create and place new assets within a project scan directory:<br> -  Supported source assets<br> -  Unsupported Source assets  | -  Asset Processor should successfully process supported source assets and generate product assets in the cache<br> -  AP should not process unsupported source assets                                                                                                                   |
| Open new asset Job Log from Jobs tab right-click context menu.          |	Opens the log file up in filesystem default program for .log files.                                                           | -  Should have no asserts when opening the log file<br> -  Job log file should not have unnecessary/unhelpful log spam<br> -  Job log file should not be empty on a job with warnings, errors, or failures                                                                               |
| Open new asset in Asset Browser from Jobs tab right-click context menu. | Open up Asset Browser<br> *Note: Editor must already be open.*                                                                | Processed assets should be discoverable and intractable within the Asset Browser.                                                                                                                                                                                                        |

### Area: Source and Product Asset GUI Navigation

**Product:**  Source Asset and Product Asset are discoverable.

**Suggested Time Box:** 10 Minutes

|Workflow                                                                           | Requests                                                                                                         | Things to Watch For                                                                                                                                                                                                                                                                                                                        |
|-----------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| From Jobs tab jump to source asset from right-click context menu.                 | Source Assets tab is opened to the the selected source asset.                                                    | Source asset should be found within the tab.                                                                                                                                                                                                                                                                                               |
| Jump to a product asset from selected asset's right-click context menu.           | Product Assets tab is opened to the the selected source asset.                                                   | Product asset should be found within the tab.                                                                                                                                                                                                                                                                                              |
| Jump back to Source Asset from selected asset's info panel "Source Asset" button. | Source Assets tab is opened to the the selected source asset.                                                    | Source asset should be found within the tab.                                                                                                                                                                                                                                                                                               |
| Reprocess asset from selected Source Asset's right-click context menu.            | -  Asset processor performs job analysis on file<br> -  Asset is reprocessed<br> -  Asset is is product cache | -  There should be no excessive job analysis scanning<br> -  There should be no excessive processing time for files<br> -  Asset Processor errors should be descriptive and actionable<br> -  Asset Processor should successfully process source files into the cache as product files<br> -  New Job Logs and Product files should be output |
| Open file in Explorer from selected asset's right-click context menu.             | File Explorer is opened to the source file's location.                                                           | -  Wrong folder is opened<br> -  Folder fails to open                                                                                                                                                                                                                                                                                      |
