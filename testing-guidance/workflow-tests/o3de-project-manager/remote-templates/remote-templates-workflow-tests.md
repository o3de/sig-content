# Remote Templates

These workflows center around testing the basic functionality of the Project Manager remote template acquisition tools.

## Documentation & Tutorials

* [Workshop: Building a Networked Game from Scratch...](https://youtu.be/4f4olmUo44k)

## Repository Configuration

A git repository must contain a `repo.json` at its root which points to templates. The `repo.json` contains a template 
list which points to the project templates within said repository.

See [O3DE-Extras repo.json](https://github.com/o3de/o3de-extras/blob/development/repo.json) as an example.
```json
"templates": [
        "https://github.com/o3de/o3de-extras.git/Templates/Multiplayer",
        "https://github.com/o3de/o3de-extras.git/Templates/Ros2ProjectTemplate"
    ]
```

Each template folder must contain a `template.json` at its root which describes the template. Including information such 
as where the template zip can be downloaded, and the zip's SHA256 hash value for security.

See [O3DE-Extras Multiplayer template.json](https://github.com/o3de/o3de-extras/blob/development/Templates/Multiplayer/template.json) as example.
```json
{
    "template_name": "Multiplayer",
    "origin": "Open 3D Engine - o3de.org",
    "origin_uri": "https://github.com/o3de/o3de-extras/releases/download/1.0/Template_Multiplayer-1.0.zip",
    "sha256": "c1d1f6e260c6121f013b4d54e213e0c7f0f99281c66ebe48bb6403a1180ee444"
}
```

## Prerequisites

* Locally installed tools for the remote source being used (git installed with credentials for GitHub, for example)
* A remote Git repository (GitHub, AWS CodeCommit, BitBucket, self-hosted, ETC) that has a remote template.
    * Examples:
       * O3DE-Extras (https://github.com/o3de/o3de-extras.git)
       * Another [O3DE](https://github.com/o3de) owned sample with a remote template
    * Your own repository URI with a template you've created.

## Common Issues

*   Template does not download properly
*   User attempts to use a template without downloading
*   Create project screen doesn't refresh to show the registered remote source

## Workflows

| Workflow                                       | Steps                                                                                                                                                                                          | Expectations                                                                                                                                                                        |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Register a remote template URL                 | 1.  Begin creating a new project<br>2.  Select Add a Remote Template<br>3.  Enter a valid URL for the project template                                                                         | *   All templates present in the entered URL display in the template list as options for installing<br>*   Cloud icon displays for newly registered templates that can be installed |
| Install a remote template                      | 1.  Register a remote template URL<br>2.  Select the desired template from the template list (in project creation)<br>3.  Install the template                                                 | *   Template downloads<br>*   Cloud icon is removed from template list                                                                                                              |
| Install remote template in custom directory    | 1.  Register a remote template URL<br>2.  Select the desired template from the template list (in project creation)<br>3.  Enter a directory other than the default<br>4.  Install the template | *   Template downloads to desired location on disk<br>*   Cloud icon is removed from template list                                                                                  |
| New project can be built using remote template | 1.  Register and install a remote template<br>2.  From the project creation screen, select the installed template<br>3.  Finish creating the project<br>4.  Build the created project          | Project successfully builds and can be launched from Project Manager                                                                                                                |
| Uninstall a template                           | 1.  Add remote template(s)<br>2.  Install one of the registered templates<br>3.  Uninstall the installed template                                                                              | *   Template displays with the cloud icon again, showing it can be installed<br>*   Template no longer exists on disk in the install location                                       |
| Remove/Unregister a template                   | 1.  Go to the remote sources tab<br>2.  Select the template URL and remove it                                                                                                                  | Uninstalled and unregistered templates no longer display in project creation                                                                                                        |
| Invalid URL                                    | 1.  Begin creating a new project<br>2.  Select Add a Remote Template<br>3.  Enter an invalid URL for the project template                                                                      | Clear error message displays explaining the issue                                                                                                                                   |
