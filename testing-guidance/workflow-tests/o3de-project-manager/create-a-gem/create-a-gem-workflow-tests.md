# Create A Gem

These workflows center around testing the basic functionality of the Project Manager gem creation tools.

## Common Issues

*   Dialogs do not open or progress as expected
*   Created gems do not immediately refresh in gem catalog list

## Workflows

### Workflow 1: Create a Gem from O3DE Project Manager

**Platforms**
* Windows
* Linux

| Workflow                                 | Steps                                                                                                                                                                                                          | Expectations                                                                                                                                                                                                                                                                                                            |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Launch Create a Gem experience           | 1.  Enter the Gem Catalog<br>2.  Select the hamburger menu in the top right corner<br>3.  Select "Create a New Gem"                                                                                            | Dialog changes to Create a New Gem step 1 "Gem Setup"                                                                                                                                                                                                                                                                   |
| Step 1 - Gem Setup Options               | 1.  Launch the Create a Gem experience<br>2.  Observe the options in Step 1                                                                                                                                    | Dialog displays options for:<br><br>*   Prebuilt Gem<br>*   GemRepo<br>*   Asset Gem<br>*   Default Gem<br>*   C++ Tool Gem<br>*   Python Tool Gem<br>*   Choose existing template                                                                                                                                      |
| Step 2 - Gem Details                     | 1.  Launch the Create a Gem experience<br>2.  Select a gem type/template<br>3.  Left click Next                                                                                                                | All fields can be edited<br>    *   Gem System Name<br>   *   Gem Display Name<br>    *   Gem Summary<br>    *   Requirements<br>    *   Target Platform(s)<br>   *   License<br>    *   License URL<br>    *   User-defined Gem Tags<br>    *   Gem Location<br>    *   Gem Icon Path<br>    *   Documentation URL<br> |
| Step 2 - Required Fields not satisfied   | 1.  Launch the Create a Gem experience<br>2.  Select a gem type/template<br>3.  Left click Next<br>4.  On step 2 page, click next without entering information into some of the required fields (marked by \*) | *   User is unable to progress without required fields and a visual indicator suggests what is missing<br>   Required Fields:<br>    *   Gem Display Name<br>    *   Gem System Name<br>    *   License<br>   *   Gem Location<br>                                                                                      |
| Step 3 - Creator Details                 | 1.  Launch the Create a Gem experience<br>2.  Select a Gem Type/Template<br>3.  Left Click Next<br>4.  Enter Details for Gem<br>5.  Left Click Next                                                            | *   User is unable to progress without required fields and a visual indicator suggests what is missing<br>   Required Fields:<br>    *   Creator Name<br>                                                                                                                                                               |
| Gem name matches existing gem            | 1.  Create two gems with the same name, but different values for description, etc.                                                                                                                             | *   No issues in using both gems together                                                                                                                                                                                                                                                                               |
| Created Gem displays in Gem Catalog      | 1.  All steps completed (1-3) for creating a new gem<br>2.  Go to Gem Catalog if not already there after creating                                                                                              | *   Gem can be found in the gem catalog and enabled/disabled                                                                                                                                                                                                                                                            |
| Platform Support - Platforms Specified   | 1.  Create a Gem, including Platform selections during Step 2<br>2.  Finish Gem Creation                                                                                                                       | * Created Gem shows the selected Platforms in its description within the Gem Catalog                                                                                                                                                                                                                                    |
| Platform Support - Platforms Unspecified | 1.  Create a Gem, without including Platform selections during Step 2<br>2.  Finish Gem Creation                                                                                                               | * Created Gem Shows no entry related to Platforms in its description within the Gem Catalog                                                                                                                                                                                                                             |


### Workflow 2: Add a remote gem from O3DE Project Manager

**Workflow Docs**
https://www.o3de.org/docs/user-guide/gems/repositories/overview/

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