# Remote Templates

These workflows center around testing the basic functionality of the Project Manager remote template acquisition tools.

## Prerequisites

*   Locally installed tools for the remote source being used (git installed with credentials for GitHub, for example)

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
