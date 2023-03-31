# Remote Projects

These workflows center around testing the basic functionality of the Project Manager remote project acquisition tools.

## Repository Configuration

A git repository must contain a `repo.json` at its root which defines it as a remote project.

See [ThirdPersonTemplate repo.json](https://github.com/o3de/ThirdPersonTemplate/blob/main/repo.json) as an example.
```json
{
    "projects": [
        "https://github.com/o3de/ThirdPersonTemplate.git"
    ]
}
```

The remote project's `project.json` contains a definition defining it as a remote project.

See [ThirdPersonTemplate project.json](https://github.com/o3de/ThirdPersonTemplate/blob/main/project.json) as an example.
```json
{
    "project_name": "ThirdPersonTemplate",
    "project_id": "{b4db7f1f-a52b-40be-86f6-22e120b9ff55}",
    "origin": "https://github.com/o3de/ThirdPersonTemplate",
    "license": "https://opensource.org/licenses/Apache-2.0, https://opensource.org/licenses/MIT, https://creativecommons.org/licenses/by/4.0/",
    "display_name": "ThirdPersonTemplate",
    "summary": "A template project of an animated and controllable character with a 3rd Person perspective camera.",
    "canonical_tags": [
        "Project"
    ],
    "user_tags": [
        "ThirdPersonTemplate"
    ],
    "icon_path": "preview.png",
    "engine": "o3de-sdk",
    "external_subdirectories": [
        "Gem"
    ],
    "restricted": "ThirdPersonTemplate",
	"origin_uri":"https://github.com/o3de/ThirdPersonTemplate.git"
}
```

## Prerequisites

* Locally installed tools for the remote source being used (git installed with credentials for GitHub, for example)
* A remote Git repository (GitHub, AWS CodeCommit, BitBucket, self-hosted, ETC) that has a remote project.
     * NewspaperDeliveryGame (https://github.com/o3de/NewspaperDeliveryGame.git)
     * ThirdPersonTemplate (https://github.com/o3de/ThirdPersonTemplate.git)
     * Another [O3DE](https://github.com/o3de) owned sample with a compatible remote project.
     * Your own repository URI with a project you've created.

## Common Issues

*   Project does not download successfully
*   Project doesn't build automatically, if that option is selected

## Workflows

| Workflow                                                               | Steps                                                                                                                                                                                                                                                                                            | Expectations                                                                                                                                                                                          |
|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Add remote project from fresh Project Manager (no projects registered) | 1.  Make sure no projects are registered (if there are already projects registered, they can be removed from the o3de\_manifest.json)<br>2.  Launch Project Manager (o3de.exe)<br>3.  Select "Add a remote project"                                                                              | Dialog opens with entry point for repo url and related options                                                                                                                                        |
| Add remote project as new project (other projects already registered)  | 1.  From the Project Manager projects list, select New Project... > Add a remote project                                                                                                                                                                                                         | Dialog opens with entry point for repo url and related options                                                                                                                                        |
| Valid URL                                                              | 1.  Add a valid url to the Repository Path field                                                                                                                                                                                                                                                 | Relevant information related to the repo populates in the dialog<br><br>*   requirements<br>*   license<br>*   download size                                                                          |
| Invalid URL                                                            | 1.  Add an invalid url to the Repository Path field                                                                                                                                                                                                                                              | A warning/clarification states that the url does not contain valid O3DE objects                                                                                                                       |
| Project/Repo requirements                                              | 1.  Valid url added to the Repository Path field                                                                                                                                                                                                                                                 | Project requirements populate in the dialog with information relevant to the entered url's project(s)                                                                                                 |
| Project/Repo License information                                       | 1.  Valid url added to the Repository Path field                                                                                                                                                                                                                                                 | License information populates in the dialog with a link for further information                                                                                                                       |
| Change download location                                               | 1.  Valid url added to the Repository Path field<br>2.  Choose the folder icon in the Install Location field<br>3.  Navigate to a new location on disk<br>4.  Check the acknowledgement field<br>5.  Click download or download and build (depending on option selected for the automatic build) | The project is downloaded into the newly defined location on disk, rather than the default                                                                                                            |
| Download with Automatic Build                                          | 1.  Valid url added to the Repository Path field<br>2.  Check the acknowledgement field<br>3.  Click Download & Build                                                                                                                                                                            | Project downloads and successfully builds upon download completion                                                                                                                                    |
| Download without Automatic Build                                       | 1.  Valid url added to the Repository Path field<br>2.  Check the acknowledgement field<br>3.  Click Download                                                                                                                                                                                    | Project downloads and is listed in Project Manager, but notifies user that it needs to be built                                                                                                       |
| User has not checked acknowledgement of requirements/license           | 1.  Valid url added to the Repository Path field<br>2.  Click Download                                                                                                                                                                                                                           | *   Notification that the acknowledgement wasn't checked<br>*   Dialog doesn't close<br>*   Download doesn't occur                                                                                    |
| Remove remote project (remove registration)                            | 1.  From project screen, select the hamburger menu for the given project<br>2.  Select "Remove Project"                                                                                                                                                                                          | Project remains on disk, but no longer displays in Project Manager                                                                                                                                    |
| Delete remote project                                                  | 1.  From project screen, select the hamburger menu for the given project<br>2.  Select "Delete Project"                                                                                                                                                                                          | Project is no longer on disk, but a download option is available in Project Manager                                                                                                                   |
| Remove repo registration                                               | 1.  Go to Engine > Remote Sources<br>2.  Remove the entry corresponding to the project                                                                                                                                                                                                           | *   If project is downloaded, it remains in the project manager but cannot be downloaded again if deleted<br>*   If the project was not already downloaded, it no longer displays in the project list |
