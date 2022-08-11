# Terrain Workflow Tests

Testing in this area revolves around the construction of Terrain surfaces, applying materials and vegetation to the terrain, and ensuring proper physics reactions with terrain. Workflows in this area will lead into each other, ultimately resulting in a saved level with physicalized terrain defined by heightmap with macro and detail materials applied.

## Project Requirements
AutomatedTesting project, or another project with Terrain/PhysX Gems enabled.

## Common Issues to Watch For

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
1. Console log errors/warnings/spam
2. Asserts

### Workflow 1: Constructing terrain

**User-facing Documentation:**

[https://o3de.org/docs/user-guide/gems/reference/environment/terrain/](https://o3de.org/docs/user-guide/gems/reference/environment/terrain/)

[https://o3de.org/docs/user-guide/components/reference/terrain/layer\_spawner/](https://o3de.org/docs/user-guide/components/reference/terrain/layer_spawner/)

[https://o3de.org/docs/user-guide/components/reference/terrain/terrain\_height\_gradient\_list/](https://o3de.org/docs/user-guide/components/reference/terrain/terrain_height_gradient_list/)  


**Product:** Saved level with basic terrain

**Suggested Time Box:** 20-30 mins

| Workflow                                       | Requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Things to Watch For                                                                            |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| Define the area for Terrain to be spawnable in | *   Add the Terrain level components:<br>    *   Terrain World<br>    *   Terrain World Renderer<br>    *   Terrain World Debugger<br>*   Adjust Min Bounds<br>*   Adjust Max Bounds<br>*   Adjust Height Query Resolution<br>*   Toggle Terrain World Debugger display options<br>*   Adjust the Rendered world size on the World Renderer (**WIP - Currently defaults to 1km**)                                                                                                                             | *   Rendering issues with base World plane<br>*   Rendering issues with Terrain World Debugger |
| Define a spawner area                          | *   Add the required Terrain components:<br>    *   Terrain Layer Spawner<br>    *   Terrain Height Gradient List<br>    *   Axis Aligned Box Shape<br>*   Setup a new Gradient generator (or several) entity to serve as a heightmap:<br>    *   Image Gradient component<br>    *   Shape component to define Gradient area (e.g. Shape Reference pinned to Spawner)<br>    *   Gradient Transform Modifier<br>*   Assign the heightmap gradient(s) to the spawner:<br>    *   Terrain Height Gradient List | *   Crash when translating generated terrain near edges of Terrain Spawner plane               |
| Define additional spawner areas                | *   Resize defined spawner areas as needed to fit additional spawner areas<br>*   Overlap/arrange terrain spawner areas, defining Layer and Sub Priorities as needed to create an interesting look                                                                                                                                                                                                                                                                                                            | *   Rendering artifacts<br>*   Seams where spawner areas meet<br>*   Priority issues           |
| Adjust Continuous LOD                          | *   In the Terrain World Renderer:<br>    *   Toggle on Continuous LOD (CLOD)<br>    *   Adjust the following:<br>        *   First LOD distance<br>        *   CLOD Distance                                                                                                                                                                                                                                                                                                                                 |                                                                                                |
| Adjust Clipmap configuration                   | *   In the Terrain World Renderer:<br>    *   Toggle on Clipmap Enabled<br>    *   Adjust the following:<br>        *   Clipmap image size<br>        *   Macro clipmap max resolution: texels/m<br>        *   Detail clipmap max resolution: texels/m<br>        *   Macro clipmap scale base<br>        *   Detail clipmap scale base<br>        *   Macro clipmap margin size: texels<br>        *   Detail clipmap margin size: texels                                                                   | *   Crash when quickly adjusting settings<br>*   Rendering issues with terrain in viewport     |
| Save Level/Spawners as Prefabs                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                |

### Workflow 2: Generating Terrain surfaces

**User-facing Documentation:**

**Product:** Saved level with terrain spawners configured to generate surfaces

**Suggested Time Box:** 15-20 mins

| Workflow                                 | Requests                                                                                                                                                                                                                                               | Things to Watch For |
|------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|
| Define the gradient for a surface area   | *   Setup a new Gradient generator (or several) entity to serve as a heightmap:<br>    *   Image Gradient<br>    *   Shape component to define Gradient area (e.g. Shape Reference component pinned to Spawner)<br>    *   Gradient Transform Modifier |                     |
| Map configured gradients to surface tags | *   Add the required component to a Terrain Layer Spawner:  <br>    *   Terrain Surface Gradient List<br>*   Add configured gradients, and assign a surface tag                                                                                        |                     |
| Adjust terrain surfaces                  | *   Adjust Surface Data Query Resolution in the Terrain World component                                                                                                                                                                                |                     |
| Save Level/Spawners as Prefabs           |                                                                                                                                                                                                                                                        |                     |

### Workflow 3: Applying Materials to Terrain

**User-facing Documentation:**

**Product:** Saved level with terrain with a macro material and detail materials

**Suggested Time Box:** 15-20 mins

| Workflow                       | Requests                                                                                                                                                                                                                                          | Things to Watch For                                                 |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| Create a Macro Material        | *   Add required component to the Terrain Layer Spawner:<br>    *   Terrain Macro Material<br>*   Configure the parameters to generate a Macro Material:<br>    *   Color Texture<br>    *   Normal Texture<br>*   Flip/Adjust normals as desired | *   Rendering issues (texture stretching, missing color data, etc.) |
| Assign Detail Materials        | *   Add required components:<br>    *   Terrain Surface Materials List<br>*   Add elements, and assign a configured tag<br>*   Specify Material Asset to map to the assigned tag                                                                  |                                                                     |
| Adjust Detail Materials        | *   Utilize various texture blend modes<br>*   Toggle Height based texture blending in the Terrain World Renderer<br>*   Adjust detail rendering controls in the Terrain World Renderer                                                           |                                                                     |
| Save Level/Spawners as Prefabs |                                                                                                                                                                                                                                                   |                                                                     |

### Workflow 4: Physicalized Terrain

**User-facing Documentation:**

[https://o3de.org/docs/user-guide/components/reference/physx/collider/](https://o3de.org/docs/user-guide/components/reference/physx/collider/)  


**Product:** Saved level with terrain with a macro material and detail materials that reacts to physics objects

**Suggested Time Box:** 15-20 mins

| Workflow                                               | Requests                                                                                                                                                                                                                                                                                                                                                        | Things to Watch For                                                                                                                                                 |
|--------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Define a physicalized area                             | *   Add required components to a Terrain Layer Spawner:<br>    *   Terrain Physics Heightfield Collider<br>    *   PhysX Heightfield Collider                                                                                                                                                                                                                   |                                                                                                                                                                     |
| Define a physicalized terrain using a physics material | *   Select a Default Surface Physics Material for the entire terrain within the Terrain Physics Heightfield Collider                                                                                                                                                                                                                                            | *   Physics material mapping not working correctly                                                                                                                  |
| Verify Terrain collision with physics objects          | *   Instantiate a collider prefab or create a new PhysX Rigid Body collider setup (see [https://o3de.org/docs/user-guide/components/reference/physx/collider/](https://o3de.org/docs/user-guide/components/reference/physx/collider/))<br>*   Position the prefab on/above the physicalized terrain<br>*   Enter/Exit Game Mode<br>*   Enter/Exit Simulate Mode |                                                                                                                                                                     |
| Save Level/Spawners as Prefabs                         |                                                                                                                                                                                                                                                                                                                                                                 |                                                                                                                                                                     |
| Open the level in ProjectName.GameLauncher.exe         |                                                                                                                                                                                                                                                                                                                                                                 | *   Missing or inconsistent data (textures, heightmap, detail materials, etc.)<br>*   Excessive load times<br>*   Large Frame Rate disparity vs Game Mode in Editor |

### Workflow 5: Large Terrain Support

**Product:** Saved level with terrain spawners configured to generate surfaces

**Suggested Time Box:** 5-10 mins

| Workflow                     | Requests                                                                                                                                                                                                                                                                                                                                                                                            | Things to Watch For                                                 |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| Define a larger terrain area | *   Proportionally increase the size of your Terrain Spawner's Shape component to the following dimensions (X,Y,Z): 32 km x 32 km x ?<br>*   In the Terrain World Renderer:<ul>    *   Increase the Mesh render distance to match, or exceed, the Terrain Spawner Shape component<br>*   Increase the Camera component's Far clip distance to match, or exceed, the Terrain Spawner Shape component | *   Rendering issues (texture stretching, missing color data, etc.) |