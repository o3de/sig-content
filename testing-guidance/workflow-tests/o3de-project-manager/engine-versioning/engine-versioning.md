# Engine Versioning Workflow Tests

Testing in this area should focus on testing around Engine Versioning throughout the O3DE tools.

## General Docs

* [TBD](https://www.o3de.org/docs/)

## General Testing Notes

**Managed Version File (Preferred Approach)**

Manual incremental version increases in a centralized file `engine.json` found in O3DE engine base directory.

* "Centralized file" is likely going to be engine.json
  * It's currently unclear which field will be used for the development increment
    * Initially planned to use O3DEVersion entry, but this is periodically used for releases, etc already
    * Working to determine if a new field will be used for this
  * Expected to update the version with any "breaking" changes

## Common Issues to Watch For

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
- Engine version doesn't propagate to a tool.
- Engine version mismatch causing erratic behavior.
- Unexpected build failures due to version mis-matches.

## Workflows

### Area: General Engine Versioning Tool Validation Workflow

**Editor Platforms:**
* Windows
* Linux

**Product:** Changing engine versions is reflected across  

**Suggested Time Box:** 30 minutes per platform.

| Workflow                                                                       | Requests                                                                                                                                                                                                           | Things to Watch For                                                                                                                                                                                                                  |
|--------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Engine Version propagates to Engine**                                        | <ol><li>Engine built.</li><li>Project created/built.</li><li>Launch the Editor.</li><li>Observe the following areas:<ul><li>Installer.</li><li>Splash Screen.</li><li>About Screen.</li></ul></li></ol>            | <ul><li>Displayed version on the various engine screens match that found in the engine version file.</li></ul>                                                                                                                       |
| **Project Engine Field Populates with All Locally Installed Engine Versions**  | <ol><li>Launch Project Manager.</li><li>Edit Project Settings for an existing project.</li><li>Click the drop-down for the Engine entry.</li></ol>                                                                 | <ul><li>All locally installed/built Engine versions are populated in the list.</li></ul>                                                                                                                                             |
| **Current Engine Version displays for projects**                               | <ol><li>Launch Project Manager.</li><li>Go to existing project settings.</li></ol>                                                                                                                                 | <ul><li>Engine entry displays current engine for given project.</li></ul>                                                                                                                                                            |
| **Supported Engine Version functions for Gems**                                | <ol><li>Confirm Gem listed current engine as supported in the gem's `gem.json` in the gem folder.</li><li>Add the gem to a project compatible with the current Engine version.</li><li>Build the project</li></ol> | <ul><li>No Compile issues with the project.</li><li>The gem functionality works as expected.</li></ul>                                                                                                                               |
| **Current Engine Name and Version (Title Bar)**                                | <ol><li>Launch Project Manager.</li><li>Observe Window Title Bar.</li></ol>                                                                                                                                        | <ul><li>Title Bar displays the Engine name and version that Project Manager was launched from.</li></ul>                                                                                                                             |
| **Engine Tab displays current Engine Version (Project Manager's source)**      | <ol><li>Launch Project Manager.</li><li>Click the Engine tab</li><li>Observe the Engine Name and Version fields.</li></ol>                                                                                         | <ul><li>Engine Name and Version match the engine that was used to launch Project Manager.</li></ul>                                                                                                                                  |
| **Update Project's Engine**                                                    | <ol><li>Launch Project Manager.</li><li>Edit Project Settings for an existing Project.</li><li>Click the drop-down for the Engine entry</li><li>Select a new eligible (local) engine from the list</li></ol>       | <ul><li>Change is visible under the project tile in the projects list.</li><li>Projects page suggests the project needs to be rebuilt.</li></ul>                                                                                     |
| **Current Engine version for Project Displays Below Project Tile**             | <ol><li>Launch Project Manager.</li><li>Note the text below the existing project tiles.</li></ol>                                                                                                                  | <ul><li>Displays the projects currently assigned Engine name and version.</li><li>Color is white if it matches the current engine and yellow if it is for a different version than what Project Manager was launched from.</li></ul> |
| **Different Engine Versions can be installed side by side**                    | <ol><li>Install the latest release using the installer.</li><li>Install the latest development installer.</li></ol>                                                                                                | <ul><li>The second install does not overwrite/uninstall the first.</li><li>Both engine versions are functional.</li></ul>                                                                                                            |
---

### Area: Engine Versioning CLI Workflows

**Example Parameters**
* `<EngineVersion>` can be replaced with something along the lines of `o3de==1.0.0`.
   * Value passed in must be a registered engine name. 

**Editor Platforms:**
* Windows
* Linux

**Suggested Time Box:** 30 minutes per platform.

| Workflow                                                                          | Requests                                                                                                                                                                                                                                                                                                                                                                                     | Things to Watch For                                                                                                                                                     |
|-----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Multiple Instances of Engine with same Name and Version**                       | <ol><li>Clone two instances of the same engine into the local directory.<ul><li>EG: `C:\o3de1` and `C:\o3de2`.</li></ul></li><li>Register both engines using `scripts/o3de register --this-engine`.</li><li>Create a project.</li><li>Register it to one of the engine options using `scripts/o3de edit-project-properties -pp <ProjectPath> --user --engine-path <PathToEngine>`.</li></ol> | <ul><li>Project successfully registers the given Engine.</li><li>This is visible in Project Manger after registry.</li><li>Project can be successfully built.</li></ul> |
| **Specify Specific Engine version for Project (global)**                          | <ol><li>Create a new project if one does not exist.</li><li>Use `scripts/o3de edit-project-properties -pp <ProjectPath> --engine-name <EngineVersion>` to assign the project version to use.</li></ol>                                                                                                                                                                                       | <ul><li>Project's `project.json` reflects the selected engine version.</li><li>Project can be successfully built.</li></ul>                                             |
| **Specify Specific Engine version for Project (local)**                           | <ol><li>Create a new project if one does not exist.</li><li>Use `scripts/o3de edit-project-properties -pp <ProjectPath> --engine-name <EngineVersion> --user` to assign the project version to use.</li></ol>                                                                                                                                                                                | <ul><li>Project's `project.json` generated in `<ProjectPath>\user\` with selected engine version.</li><li>Project can be successfully built.</li></ul>                  |
---



