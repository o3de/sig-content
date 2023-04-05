# Gem Workflow Tests

These workflows center around testing the basic functionality of the Project Manager's gem tooling. Including 
Gem Creation, Gem Catalog, and Gem Remote Sources Manager. 

## General Docs

* [O3DE Project Configuration](https://www.o3de.org/docs/user-guide/project-config/)
* [O3DE Project Manager](https://www.o3de.org/docs/user-guide/project-config/project-manager/)

## Common Issues

*   Dialogs do not open or progress as expected.
*   Created gems do not immediately refresh in **Gem Catalog** list.

## Workflows

### Area: Gem Catalog Experience


**Platforms:**
* Windows
* Linux

**Product:** A project with modified gems.

**Suggested Time Box:** 15 minutes per platform.

| Workflow                          | Requests                                                                                                                                                                                                                                                                                                                                                          | Things to Watch For                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Find a gem in the Gem Catalog** | <ol><li>Open the **O3DE Project Manager**.</li><li>Select the **Gems** button from the **O3DE Project Manager** navigation ribbon.</li><li>Apply a simple search string to the **Search Bar**.<ul><li>Something simple like `physics`.</li></ul></li><li>Apply **Search Filters** to discover a gem that you need from the different filter categories.</li></ol> | <ul><li>**Gem Catalog** can be opened.</li><li>Applying/removing a search string in the **Search Bar** displays the expected gems.</li><li>Applying/removing a **Search Filter** displays the expected gems.</li><li>The expected **Gem Image**, **Gem Name**, **Gem Summary**, and **Gem Status** are displayed for each gem's **Gem Entry** in the **Gem List**.<ul><li>**Gem Images** can be seen for given gems and have the expected quality.</li><li>Default **Gem Image** is present when there isn't a specific one authored for the gem.</li><li>The **Gem Name** field in the **Gem List** can be resized by clicking on the separator between the **Gem Name** & **Gem Summary** header titles in the **Gem List**.</li></ul></li></ul> |
| **Edit a gem**                    | <ol><li>Find a gem you want to edit in the **Gem Catalog** using your desired search & filter method.</li><li>Select the gem you want to edit.</li><li>Click the **Edit** button in the **Gem Details** panel.</li><li>Modify the **Gem Details** you want to edit and click next.</li><li>Modify **Creator Details** you want to edit and confirm.</li></ol>     | <ul><li>When gem is selected the gem's **Gem Details**, **Creator Details**, and **Gem Dependencies** are displayed in the **Gem Details** panel.</li><li>Links in the **Gem Details** panel for a selected gem are clickable and respond as expected.<ul><li>Clicking on a **Depending Gem** in the **Gem Details** panel will navigate to and display the info for the **Depending Gem** that was clicked if it is available.</li></ul></li><li>**Gem Details** can be modified and are reflected when changed in the **Gem Catalog** as well as in the gem's `gem.json` file.</li><li>**Gem Details** can be modified and are reflected when changed in the **Creator Catalog** as well as in the gem's `gem.json` file.</li></ul>              |

---

### Workflow: Create a Gem from O3DE Project Manager

### New Gem Template

* Prebuilt Gem
* GemRepo
* Asset Gem
* Default Gem
* C++ Tool Gem
* Python Tool Gem
* Choose existing template

#### Gem Details Editable Fields
 
* Gem System Name [*Required]
* Gem Display Name [*Required]
* Gem Summary
* Requirements
* Target Platform(s)
* License [*Required]
* License URL
* User-defined Gem Tags
* Gem Location [*Required] [o3de/o3de#13736](https://github.com/o3de/o3de/issues/13736) 
* Gem Icon Path [o3de/o3de#15540](https://github.com/o3de/o3de/issues/15540)
* Documentation URL

#### Creator Details Editable Fields

* Creator Name [*Required]
* Origin URL
* Repository URL

**Target Platforms:**

* Windows
* Linux
* iOS
* Android
* All Platforms

**Platforms**
* Windows
* Linux

**Product:** A newly created gem that can interact with the **Gem Catalog**, is present on the file system, and be added
to a project.

**Suggested Time Box:** 30 minutes per platform.

| Workflow                                 | Steps                                                                                                                                                                                                                                                                                                                                                                                    | Expectations                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Create a Gem Experience                  | <ol><li>Enter the Gem Catalog.</li><li>Select the hamburger menu in the top right corner.</li><li>Select **Create New Gem**.</li><li>Select a gem **type** or **template** that you want to create and click next.</li><li>Populate the gem details fields and click next.</li><li>Populate the Creator Details and click next.</li><li>Return to **Gem Catalog** and refresh.</li></ol> | <ul><li>A new gem can be created using all [New Gem Templates](#new-gem-template).</li><li>All [Gem Details Editable Fields](#gem-details-editable-fields) & [Creator Details Editable Fields](#create-details-editable-fields) can be modified.</li><li>If [Required Gem Details Fields](#gem-details-editable-fields) or [Creator Details Editable Fields](#create-details-editable-fields) are not populated with valid data the user cannot proceed and a visual indicator informs the user of the required fields.</li><li>Gem folder is created in the target destination with the expected type or template.</li><li>Created Gem displays in Gem Catalog.</li></ul> |
| Gem name matches existing gem            | <ol><li>Create two gems with the same name, but different values for description, etc.</li></ol>                                                                                                                                                                                                                                                                                         | <ul><li>No issues in using both gems together.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Platform Support - Platforms Specified   | <ol><li>Create a Gem and include desired [Target Platforms](#target-platforms) selections on the **Gem Details** panel.</li></ol>                                                                                                                                                                                                                                                        | <ul><li>Created Gem shows the selected Platforms in its description within the **Gem Catalog**.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Platform Support - Platforms Unspecified | <ol><li>Create a Gem, without including any desired [Target Platforms](#target-platforms)</li></ol>                                                                                                                                                                                                                                                                                      | <ul><li>Created Gem Shows no entry related to Platforms in its description within the **Gem Catalog**.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |


### Workflow: Add a remote gem from O3DE Project Manager

**Workflow Docs**
[O3DE Gem Repositories](https://www.o3de.org/docs/user-guide/gems/repositories/overview/)

**Repository Configuration**

A git repository must contain a `repo.json` at its root which points to templates. The `repo.json` contains a gems 
list which points to the remote gems within said repository.

See [sample-code-gems repo.json](https://github.com/o3de/sample-code-gems/blob/main/repo.json) as an example.

```json
{
    "gems": [
        "https://raw.githubusercontent.com/o3de/sample-code-gems/main/atom_gems/AtomTutorials",
        "https://raw.githubusercontent.com/o3de/sample-code-gems/main/cpp_gems/ShapeExample",
        "https://raw.githubusercontent.com/o3de/sample-code-gems/main/py_gems/PyShapeExample"
    ]
}
```

**Workflow Requirements**
* A built project to add a remote gem to (a blank template project is acceptable).
* Locally installed tools for the remote source being used (git installed with credentials for GitHub, for example)
* A remote Git repository (GitHub, AWS CodeCommit, BitBucket, self-hosted, ETC) that has a remote project.
  * Examples: 
    * sample-code-gems (https://github.com/o3de/sample-code-gems.git)
    * O3DE Canonical Gems (https://canonical.o3de.org) [o3de/o3de#15557](https://github.com/o3de/o3de/issues/15557)
    * Another [O3DE](https://github.com/o3de) owned sample with compatible remote gems.
  * Your own repository URI with a remote gem you've created.

**Platforms**
* Windows
* Linux

**Product:** A built project with a remote gem added to it.

**Suggested Timebox:** 30 minutes per platform

| Workflow                                                 | Requests                                                                                                                                                                                                                                                                                                                                                                                                                | Things to Watch For                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Add **Remote Gem(s)** to **Gem Remote Sources Manager**. | <ol><li>Open O3DE Project Manager.</li><li>Select **Gems** from the O3DE Project Manager Ribbon.</li><li>Open **Gem Remote Sources Manager**. <ul><li>`Gem Catalog Hamburger Menu → Show Gem Repos`</li></ul></li><li>Add your remote gem repository.<ul><li>Click **Add Repository** button.</li><li>Add your remote gem URI and click the **Add** button in the **Add a User Repository** dialog.</li></ul></li></ol> | <ul><li>**Gem Remote Sources Manager** lists the remote repositories.</li><li>Remote gems are displayed in **Gem Catalog** only when Remote Source is added to the **Gem Remote Sources Manager**.</li><li>**Gem Remote Sources Manager** can be refreshed.</li><li>A **Remote Source** can be deleted from the **Gem Remote Sources Manager.**</li><li>Gems that have had their **Remote Source** removed from the **Remote Sources Manager** do not display in the **Gem Catalog**.</ul>                               |
| Add **Remote Gem** to an **O3DE Project**.               | <ol><li>Add **Remote Gem(s)** to the **Gem Remote Sources Manager.**</li><li>Configure your project to add your **Remote Gem(s)**.</li><li>Build and open your project in the O3DE Editor.</li></ol>                                                                                                                                                                                                                    | <ul><li>**Remote Gem(s)** can be added & removed from an **O3DE Project**.</li><li>Known good **Remote Gem(s)** should build.</li><li>**Remote Gem(s)** content can be interacted with from the O3DE Editor. <ul><li>EG: Assets are processed and available in asset manager if assets are present.</li><li>EG: Editor Entity Components can be added if Editor Entity Components are present.</li><li>EG: Script Canvas nodes can be added to a graph if Script Canvas nodes are present in the gem.</li></ul></li></ul>|
---

