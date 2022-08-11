**Dynamic Vegetation**
----------------------

Testing in this area should revolve primarily around component configuration to create an interesting instance spawner setup

## Project Requirements
AutomatedTesting project, or another project with Atom and all Vegetation system Gems

## Common Issues to Watch For

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
1. Console log errors/warnings/spam
2. Asserts

### Workflow 1: Construction/configuration of an instance spawner

**User-facing Documentation: [https://www.o3de.org/docs/user-guide/gems/reference/environment/vegetation/procedures/](https://www.o3de.org/docs/user-guide/gems/reference/environment/vegetation/procedures/)**

**Product:** Prefab of constructed instance spawner, Saved/exported level using instance spawner

**Suggested Time Box:** 30 mins

| Workflow                                                                     | Requests                                                                                                                                                                                                                                                | Things to Watch For                                                                                 | Testing Notes |
|------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------|
| Construct several different planting surfaces                                | *   Use a mixture of terrain surfaces and emitter types using different surface tags<br>*   Create custom surface tags in Asset Editor                                                                                                                  | *   Component dependency issues<br>*   Surface Tag availability                                     |               |
| Create one/several instance spawner area setups                              | *   Use multiple descriptors on a single spawner<br>*   Use varying spawner/blocker/blender areas<br>*   Use a mixture of Empty/Prefab spawner types<br>*   Configure descriptors to plant on a variety of previously created surfaces<br>*   Undo/Redo | *   Component dependency issues<br>*   Spawning issues                                              |               |
| Add Filters/Modifiers to configure planting and create an interesting layout | *   Use a variety of Gradient Generators/Modifiers to drive Filters/Modifiers<br>*   Undo/Redo<br>*   Move camera large distances around viewport<br>*   Enter/Exit Game Mode                                                                           | *   Spawning/despawning issues<br>*   Component dependency issues                                   |               |
| Add Vegetation Debugger/System Settings components via Level Inspector       | *   Override global system settings to affect your created areas (e.g. Density/Sector Size)<br>*   Toggle on Debug Stats/Sector info<br>*   Clear Performance Log and edit small parts of setup                                                         | *   Inaccurate debug stats<br>*   Spawning/despawning issues<br>*   Unintended sector re-processing |               |
| Save constructed surfaces + instance spawner areas to a prefab               |                                                                                                                                                                                                                                                         | *   Missing components<br>*   Configuration changes                                                 |               |
| Instantiate a copy of saved prefab                                           |                                                                                                                                                                                                                                                         | *   Missing components<br>*   Configuration changes                                                 |               |
| Save level                                                                   |                                                                                                                                                                                                                                                         | *   Asset Processor errors with level processing                                                    |               |
| Load level in Launcher                                                       | *   Load on at least one supported platform besides Windows                                                                                                                                                                                             | *   Spawning/despawning issues<br>*   Performance issues                                            |               |
