### Summary:
O3DE users don't have a straight-forward, accessible and customizable way to export their projects for release. This can make the process of preparing release packages of projects time consuming and error prone. This RFC proposes a new O3DE command line interface sub-command called `export-project` that simplifies the process by automating that work. The command is also setup to run custom Python scripts.

### What is the relevance of this feature?
The current process in O3DE for creating and then exporting a project to share with others takes a minimum of 12 steps.  O3DE users routinely struggle to find and follow these steps. A common piece of feedback from O3DE evaluators is the difficulty in sharing their work with others.

These 12 steps are outlined in the Example Usage section of the RFC. That section also demonstrates how the `export-project` command reduces the steps down to 5.

This RFC focuses on improving the following use cases for exporting:
* Standalone Release builds (Windows, Linux)
* Mobile cross-compilation (Android, iOS)
* Network projects (contains Server, Client, and Unified launcher)

It is also possible that a larger game development team requires their own build and deployment process. `export-project`  should be flexible such that they are able to incorporate their requirements into the process. Some examples of this are:

* Signing packages
* Generating certificates
* Running validation steps
* Storing Backups

When complete, a new CLI will be available that can be used to setup and automate the build steps for the project, and automate most of the repetitive tasks of building the project. This CLI can act as stepping stone to integrate a project export UI accessible from Project Manager, making the process easier for newcomers to the engine.

### Feature design description:
#### Terminology
* CLI - Command Line Interface
* Host - The user's platform which is initiating the export process
* Target - A desired destination platform, which can be different from the user's current platform
* Standalone build - An assembled O3DE project that can be distributed to other machines
* Export - The process of creating a standalone build of a project
* Auxiliary files - Files that are not bundled, and are not binaries. Some examples include settings registry files and license text files.
* Archive - A compressed package (either of the release layout directory, or a sub-folder within) meant for distribution to a Target platform (i.e. ZIP for windows, APK for Android, etc.)
* Export script - a Python script run by `export-project`, that is intended to automate the process of building and packaging a project, based on a specific use case

#### Command Overview
An `export-project` O3DE CLI command is added which runs an export script.  Such a script can automate building, processing assets, bundling, and copying to an output folder. For export scripts provided with O3DE, these scripts will create the necessary release directory structure for each platform, but will not create archives nor deploy to external devices or services. That will be left to users to extend as needed.

`export-project` is a sub-command via `o3de.bat` or `o3de.py` because the task of exporting projects is logically grouped with other project related commands found in the CLI, benefiting from discoverability. It also helps with parameter management: `o3de.py`  is setup to be aware of things like the engine folder.

`export-project`'s main priority is a correct release directory structure. Archiving and deploying to devices and services will likely be separate CLI tools and are not covered here.

#### Example Usage
Suppose a game studio wants to make a game like [Newspaper Delivery Game](https://github.com/o3de/NewspaperDeliveryGame). That project has only 2 scenes, a main menu, and a game scene. It has one player character asset, a few models, such as cars, houses, trees, and roads, and 1 level file.

##### Before RFC Changes
As of January/February 2023 to get such a project ready for release with a standalone Game Launcher only, the user must manually do the following:

1. Create the project, populate it with content and author the code to drive the project's logic.
2. Build profile non-monolithic Editor & tools for the Host platform.
3. Process assets for the project with the profile Asset Processor (GUI or Batch) for the Target platform.
4. Generate or update seedlists if required (for more info see [this article on the asset bundler](https://www.o3de.org/docs/user-guide/packaging/asset-bundler/overview/)).
5. Bundle assets using seedlists for the project with the profile Asset Bundler (GUI or Batch).
6. Build release monolithic game project launcher for the Target platform.
7. Create the release layout folder directory.
8. Place binaries in the directory layout.
9. Place asset bundles in the directory layout.
10. Place auxiliary files in the directory layout.
11. Use an external archiving tool to create a zip file.
12. Put the zipped archive on [itch.io](http://itch.io/) to share it with people who want to play their game jam game.

##### After RFC Changes
With the `export-project`  command, the user would only need to do the following:
1. Create the project, populate it with content and author the code to drive the project's logic
2. Run an o3de project export command for a standalone monolithic release Windows build:
```
cd <O3DE_INSTALL_DIR>
scripts\o3de.bat export-project --export-script C:\workspace\projects\NewspaperDeliveryGame\ExportScripts\export_standalone_monolithic.py
```
3. Get this release layout output
![image](./rfc-123-project-export-cli-images/release_layout_example.png)
4. Use an external archiving tool to create a zip file.
5. Put the zipped archive on [itch.io](http://itch.io/) to share it with people who want to play their game jam game.

Between both approaches, these steps remain the same:
* Creating the project (step 1)
* Preparing the zip file (steps 11 and 4 respectively)
* Distributing on itch.io (steps 12 and 5 respectively)

### Technical design description:
#### Parameters
The initial list of parameters for this iteration of the export-project  command are:

- required
    - export-script - path to a python file encoding the desired export process to run for the project.
        - The user can expect o3de `export-project` helper APIs and variables to be available in their scripts without extra work.
- optional
    - project-path *
    - third-party-packages-path *
    - game-seed-files **
    - Asset / game content / game rules / binary version number
    - output-path
    - cmake-tool-configure-args ***
    - cmake-project-configure-args ***

*These arguments are optional from the user's perspective but required for processing the export script, and `export-project`  will attempt to infer a value if one is not given
**If not given, then a default is generated
***For any optional argument using `nargs` , this means that a list of arguments for a given toolchain (like CMake) can be passed along to the appropriate stage in the pipeline.

At a high level, the `export_project.py`  script imports the export script via `importlib` as a module to execute, and adjusts the system path to include the O3DE CLI and the script's local directory. It also processes CLI arguments, and stores them in an o3de_export object which is accessible to the user by appropriate APIs.

#### Export Scripts
The end result of exporting a project will consist of a directory layout of game binaries and packaged game content (asset bundles). A project ready for release is this folder directory placed into the exporting structure used for the intended release method (IPA for iOS, APK for Android, etc). Further processing can be handled by users.

In order to use the project export tools, the user supplies an export script file. Helper APIs and CLI parameters are accessible to the user, with no setup code required.

To make this easier, this RFC proposes providing standard scripts, which both provide default engine export functionality, and reference code for users to adapt to their own needs. Export scripts in projects can be defined in the `<project-root>/ExportScripts` directory. 

Similar to the `My Documents` folder on Windows, the `ExportScripts` folder is not strictly required for users, and they can place the scripts wherever they wish, but it comes with some conveniences. For instance, The Asset Processor should be provided an exclusion rule to not look at files in `<project-root>/ExportScripts`. For O3DE project templates, example scripts are placed in the `ExportScripts` directory, and it should be treated as common convention.

The following examples focus on use case 1 (Standalone release builds for windows specifically):

- This is an example of a very simplified export script (useful for gamejam teams):
```python
"""
Copyright (c) Contributors to the Open 3D Engine Project.
For complete copyright and license terms please see the LICENSE at the root of this distribution.
SPDX-License-Identifier: Apache-2.0 OR MIT
"""
import sys
import os
 
# User scripts can import the export API regardless of folder location
import o3de.export_project as exp
 
# User scripts can also access o3de default variables that are available before the script is run via o3de_export
 
exp.export_standalone_monolithic(platform= "Windows", build_mode = o3de_export.build_mode,
                                 project_path= o3de_export.project_path,
                                 engine_path = o3de_export.engine_path)
```
- This is an example of a slightly complex export script for standalone monolithic builds (useful for teams that need to tweak the build process):
```python
"""
Copyright (c) Contributors to the Open 3D Engine Project.
For complete copyright and license terms please see the LICENSE at the root of this distribution.
SPDX-License-Identifier: Apache-2.0 OR MIT
"""
import sys
import os
 
# User scripts can import the export API regardless of folder location
import o3de.export_project as exp
 
# User scripts can also access o3de default variables that are available before the script is run, via o3de_export
# o3de_export stores all CLI arguments passed in to the export_project command
 
# This is currently needed to access necessary O3DE toolchain for rest of the export process
exp.build_engine_tools(cwd = o3de_export.engine_path, compiler = o3de_export.args.compiler)
 
exp.process_assets(platform = "pc", project_path = o3de_export.project_path)
 
exp.bundle_assets(seedlist = o3de_export.seedlist)
 
exp.prepare_project(cmake_build_mode = o3de_export.build_mode ,cmake_gen = o3de_export.args.cmake_gen, is_monolithic = True, third_party_path = o3de_export.third_party_path)
 
exp.build_project(target = "install", config = o3de_export.build_mode, build = "build\\mono")
 
exp.copy_release_dir(dest= o3de_export.output_path)
```

#### Helper API
Currently all API functions should reside in `export_project.py`.

This RFC proposes a hierarchical approach to API design, where larger composite functions are composed of smaller granular functions, but both kinds are available to users.

The simplest function is `process_command` , which is a wrapper for `subprocess.Popen`, and takes as input a string list of arguments. It should provide options for logging verbosity.

From there we can provide helper functions building on `process_command` , such as:
* `cmake_build`
* `process_assets`
* `bundle_assets`
* `copy_release_directory`
* `cmake_prepare`
* etc...

These in turn can be composed into larger composite functions, which can be used at a macro level for convenience, such as:

* `export_standalone_monolithic`

### What are the advantages of the feature?
- `export_project` will be easy to extend with future use cases.
    -  Changes to export scripts can be made without affecting the CLI tool and unrelated export scripts.
- Allows others to integrate with their own automation, with the power behind the existing Python ecosystem in O3DE.
- Easy to extend functionality of the export script for user needs.

### What are the disadvantages of the feature?
- Adding or modifying scripts will require some knowledge of Python.
    - This process can be eased with excellent documentation, and sample scripts
- As exporting is based on existing systems, changes in those systems can affect the export scripts
    - For example, if asset bundling/processing changes, or CMake building algorithm changes expected structure, etc.
    - Though these are not caused by `export-project`, isolating export scripts can help reduce blast radius.
    - When considering automated tests, this could be an advantage from a QA perspective. The tests for this tool can be leveraged as a way to check if there are significant structural changes that affect building a game.

### How will this be implemented or integrated into the O3DE environment?
We anticipate the following directory and file changes for O3DE Engine:

* `scripts/o3de`
    * `o3de`
        * `+ export_project.py`
    *  `Platform/Windows/ExportScripts`
        * `+ export_simple.py`
        * `+ export_standalone_monolithic.py`
        * ...
    *  `Platform/Linux/ExportScripts`
        * `+ export_simple.py`
        * `+ export_standalone_monolithic.py`
        * ...
    * `Platform/...`

Providing these options for the scripts is convenient for new users with simpler projects to utilize project export features without needing to write their own.

When a user creates a new project, we should modify the project template such that it includes the following as well:

* `<project_root>`
    * `ExportScripts`
        * `+ export_simple_example.py`
        * `+ export_standard_monolithic_example.py`

These example scripts will be annotated with helpful comments to guide the user on how to extend the script for their own usage.

### Are there any alternatives to this feature?
- There is an alternate approach to handling export scripts via JSON
    - Requiring a JSON based workflow would mimic the experience of the Jenkins build pipeline, but this has 2 drawbacks:
        1. A separate schema must be proposed, and processing the logic is far more complex than allowing the user to pass in a custom script.
        2. Such a system, if not warranted is too restrictive and brittle for O3DE users, and would frustrate their process.
            a. And in the current proposal, a script can be provided as a template to cover common use cases. 
    - It's also a 2-way door. A transition is possible, if it proves necessary in the future. This is the simpler alternative.
        - The JSON method may be attractive later on for Project Manager UX, but fulfilling the proposal in this RFC will make that task easier as well.
- O3DE users can continue to build their project manually using the existing tools, without taking advantage of the proposed features in this RFC.
    - For some teams, this may be the preferred route if they already have a setup, in which case the introduction of the `export-project` command should not impede them
    - But in general, the manual process is too cumbersome for the reasons outlined earlier in this RFC.

### How will users learn this feature?
- Anyone who uses the `o3de.(bat|sh|py)` CLI tools will be able to discover the `export-project` command via the `--help` functionality in the CLI.
- Documentation must be updated to fully describe the CLI feature and example scripts that can be used.
    - This article also needs to be updated: https://www.o3de.org/docs/user-guide/packaging/windows-release-builds/

### Are there any open questions?
- Should the tool show progress?
    - It should have some indication of progress. Feedback should be active, because the build process can take a while (even an hour if building a large project from scratch).
    - It can emit logs coming from the tools/steps currently running (filtered by priority), and have a heartbeat indicating the script is still running.
    - Also indicate the totals steps in the process, and a fraction of how much is completed (i.e. 2/10 steps completed)
        - Any timeout should be user specified. Default is no time-outs.
    - There should be helper functions that let users emit progress points to project export tool
- What should be considered out of scope?
    - For advanced users, they can use this new framework to learn the basics of creating bundled release builds of their games, but they will likely need to build their own toolchain using the same underlying steps the proposed framework follows.
    - This RFC will only focus on export scripts that produce the release layout directory structure for a project. It will not consider any additional deployment steps beyond that, such as zip files, publishing to app store, or mobile deployment. This is to avoid feature creep, as the project-export's aim is to create a reliable release layout structure. Additional CLI tooling can always be made to address deployment concerns, or users can extend the scripts themselves.
    - It is not the responsibility of the project-export framework to make sure the user has CMake and a compiler (or other such tools) installed. That is part of onboarding process to O3DE
    - Although discussions of UI and/or Editor buttons for facilitating Project Exports are out of scope, it would be a natural extension to leverage the CLI tool to power that UI. That will be for a separate RFC however.
