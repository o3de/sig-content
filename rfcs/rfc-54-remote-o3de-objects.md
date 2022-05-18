### Summary

Sharing work, for example Projects that rely on external gems, or Templates is currently a manual process of gathering the required objects and registering them with the O3DE engine. A system to download Gems from O3DE repositories currently exist, extending this to all object types, and being able to gather dependencies from repositories would simplify the sharing of projects and other O3DE objects.

### What is the relevance of this feature?

Having a way to easily share Projects/Gems/Templates that users create without this feature requires a user to download the required objects, extract the files to a location and register them using the O3DE CLI, which requires knowledge of the correct arguments. Currently it is possible to add an O3DE repository which contains downloadable gems, and download and register the Gems through the Project Manager UI. Extending this to work with Projects and Templates means it would also be possible to download Projects and Templates through the Project Manager, including having a repository containing Gems, Projects and Templates. This means that a repository could contain a Gem and a sample project, all accessable and downloadable through the Project Manager UI simplifying the process of exploring new projects and gems. As with the way Project Manager works for current features, this will be implemented in Python with Project Manager being a simple UI. This also means the downloading of objects is scriptable if needed downloading objects from the network or the internet

### Feature design description

The O3DE CLI scripts and Project Manager already has the ability to add O3DE repositories and display the available Gems within that repository. The user can then select a Gem to download and register making it available for use in projects. This feature will extend the current Gem Repositories feature to Projects and Templates. This will include extending the Python scripts to support listing the available Projects and Templates and then the ability to download and register a named Project/Template. Following this, changes to Project Manager to be able to list Projects and Templates that are within added repositories and being able to trigger the download and registration of those objects. This should also be able to detect any dependencies and ask the user whether these should be downloaded in order to use the selected object. Since projects are larger for gems the scope of this work also includes the basic ability to attempt to detect and resume incomplete downloads, and have the currently downloading objects in a more accessible location. The priority of this work will be given to downloading projects, but will also cover Templates.

### Technical design description

In order to carry out this work, the following additions/changes to the current remote gem system will be made:
- Editing the functions in scripts/o3de/o3de/download.py to make sure that O3DE objects are downloaded and registered successfully. Generalizing the functions to work for all objects using a filter on type.
- Extend download functionality in scripts/o3de/o3de/util.py to attempt to detect and resume downloads that did not complete.
- Editing the Gem Catalog in Project Manager (Code/Tools/ProjectManager/Source/GemCatalog) to be more generalized. This includes being able to view the Catalog without being in the context of editing a project, and also adding a filter for the O3DE object type, so it can be filtered on Gems/Projects/Templates. When going into the Gem Catalog for a project it should be fixed to filter on Gems.
- Changes to addg additional entry points in Project Manager to register repositories and download objects. For example being able to register a repository from the Projects screen, and also being able to view and download projects from that screen.

An example layout for a repository containing 2 gems and 1 project would be:
```
{
    "repo_name": "RepositoryName",
    "origin": "CreatorName",
    "repo_uri": "http://localhost:8080/",
    "summary": "Example Repository",
    "additional_info": "Example repository hosted on local http server.",
    "gems": [
        "http://localhost:8080/TestGem",
        "http://localhost:8080/TestGem2"
    ],
    "projects": [
        "http://localhost:8080/TestProject"
    ]
}
```

### What are the advantages of the feature?

- Easier set up of any existing O3DE object that has been set up to use this system, removing the need to manually download, extract and register new objects
- Functionality will be implemented in the o3de Python scripts and used in Project Manager so it will be easy to use parts of this for scripting the set up of team members.

### What are the disadvantages of the feature?

If the UI is designed to be different for downloading Projects compared to Gems and templates, this will increase the complexity of the Project Manager.

### How will this be implemented or integrated into the O3DE environment?

This will follow the current remote gems design and be implemented within the Python scripts for the backend, with Project Manager acting as a UI calling python to manage the Repositories and initiating the download and registering of objects.

### Are there any alternatives to this feature?

The alternative other than following the manual steps to register objects, is to just fix the CLI to register these objects. This will be then providing additional scripts and documentation on how to use the functionality to automate the process, however, this still requires more knowledge of the available scripts than using the Project Manager UI.

### How will users learn this feature?

This feature will be announced in the Discord channel (sig-content) and this RFC will be posted to get preliminary notice. Once the feature is submitted, a blog post will be written to properly announce it pointing to how it will solve some common use cases. The blog post will point to documentation to help developers get started.

Documentation will be created detailing how to use the individual functions through Python for scripting, and additional documentation for the UI changes to Project Manager. However for the UI, it should also be able to guide the user through set up without the use of documentation.

### Are there any open questions?

No.
