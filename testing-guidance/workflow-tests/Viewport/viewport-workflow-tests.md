# Viewport Workflow Tests
​
Testing in this area should focus on basic Viewport functionality.
​
## Common Issues to Watch For
​
Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.

1. Console log errors/warnings/spam
2. Asserts
​
### Platforms:​

- Windows
- Linux​
​
### Documentation pages:​

* [Working with the 3D Viewport](https://www.o3de.org/docs/user-guide/editor/Viewport/)
* [Reference Spaces](https://www.o3de.org/docs/user-guide/editor/Viewport/reference-spaces/)
* [Group Selection](https://www.o3de.org/docs/user-guide/editor/Viewport/group-selection/)
* [Advanced 3D Viewport Operations](https://www.o3de.org/docs/user-guide/editor/Viewport/advanced-operations/)​
​
### Workflow 1: Camera control 
​
**Product:** 
A Level with multiple entities with a mesh component and assigned shaderball model asset.
​
**Suggested Time Box:** 5-10 mins

| Workflow                                                               | Requests																																																																						   | Things to Watch For                                                                                                                                                          |
|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Navigate level with camera controls									 | <ol><li>Select the Viewport.</li><li>Navigate the camera with `W`, `A`, `S`, `D`, `Q`, `E`. Hold `shift` for fast move.</li><li>`Right-click and drag` to rotate the camera.</li><li>Press `MMB and drag` to pan up and down.</li><li>Scroll up and down using the `MMB`, to zoom in and out.</li><li>`ALT+LMB` to orbit.</li><li>`ALT+MMB` or `ALT+Scroll` to Pan.</li><li>`ALT+RMB` to Dolly.</li></ol>| <ul><li>Testing clicking in and out of the Viewport while moving the camera</li></ul>|
| Be this camera option													 | <ol><li>Select the camera entity or create one.</li><li>In the Entity Inspector, press the `Be this camera` button.</li><li>Move the camera around.</li><li>Press the Return to default editor camera button.</li></ol>																		   | 																																											  |
| Viewport camera selector												 | <ol><li>Open the Viewport Camera Selector (Tools > Viewport > Viewport Camera Selector).</li><li>`Right-click` in the Viewport and select "Create camera entity from view".</li><li>Navigate to a different location in the Viewport and create another camera.</li><li>Navigate through the available cameras in the Viewport Camera Selector while monitoring the Viewport.</li></ol>|																						  |
---

### Workflow 2: Viewport options  
​
**Product:** 
A Level with multiple entities with a mesh component and assigned shaderball model asset.
​
**Suggested Time Box:** 20 mins

| Workflow                                                               | Requests																																																																						   | Things to Watch For                                                                                                                                                          |
|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hide/Unhide															 | <ol><li>Select an entity with mesh component in the Entity Outliner and press the `eye` icon.</li><li>Select the Show All menu option (Edit > Show All).</li><li>Select the entity in the Viewport.</li><li>Press `H` to hide the entity.</li><li>Press `Ctrl+SHIFT+H` to unhide the entity.</li></ol>|<ul><li>Correct entity Hide/Unhide functionality.</li><li>Shortcut functionality.</li></ul>																			  |
| Lock/Unlock															 | <ol><li>Select an entity with mesh component in the Entity Outliner, press the `lock` icon.</li><li>Attempt to select the entity in the Viewport.</li><li>Select the Unlock All menu option (Edit > Unlock All).</li><li>Select the entity in the Viewport and press `L` to lock the entity.</li><li>Press `Ctrl+SHIFT+L` to unlock all entities.</li></ol>|<ul><li>Correct entity Lock/Unlock functionality.</li><li>Shortcut functionality.</li></ul>						  |
| Go to position														 | <ol><li>Select View > Viewport > Go to Position...</li><li>Input coordinates.</li></ol>																																																		   | <ul><li>Camera responds as expected to the used control actions.</li></ul>																									  |
| Find selected enitites in Viewport									 | <ol><li>Select an entity and move the camera away from it.</li><li>Select View > Viewport > Find selected enitites in Viewport or use the `Z` key.</li></ol>																																	   | <ul><li>Position on the selected enity.</li></ul>																															  |
| Save Location/Go to location											 | <ol><li>Select View > Viewport > Save Location > Save location 1.</li><li>Move the camera around.</li><li>Select View > Viewport > Go to location > Go to location 1.</li><li>Use `Ctrl+F<1-12>` to save multiple bookmarks and load them with `Shift+F<1-12>`.</li></ol>					   | <ul><li>Saving and loading bookmarked locations.</li></ul>																													  |
| Component Mode Switcher												 | <ol><li>Select an entity and add multiple components with component mode(e.g. Spline, PhysX collider, WhiteBox).</li><li>Enter the component mode through the switcher in the upper left corner of the Viewport.</li><li>Switch between component modes using the switcher.</li></ol>		   | <ul><li>Switchers added to the Viewport and functioning.</li></ul>																											  |
| Stress test															 | <ol><li>Select an entity with mesh component and duplicate it (`Ctrl+D`) so you will have around 200 entities.</li><li>Select all entities in the level and manipulate them using the Transform mode widgets.</li></ol>																		   | <ul><li>Any drops in Editor stability, errors or framerate drops.</li></ul>																								  |
| Field of View/Camera speed scale 										 | <ol><li>Select dropdown menu next to the camera icon on the Viewport Toolbar and change the value of the Field of view.</li><li>Look around with the camera.</li><li>Select the camera speed scale under the FOV, change the value and move the camera around.</li></ol>						   | <ul><li>FOV and camera speed adjusting.</li></ul>																															  |
| Toggle between states													 | <ol><li>Select dropdown menu next to the bug icon on the Viewport Toolbar and select Full.</li><li>Switch between all of the modes.</li><li>Press the bug icon multiple times to switch between the modes.</li></ol>																			   |																																											  |
| Show Helpers															 | <ol><li>Select dropdown menu next to the eye icon on the Viewport Toolbar and select show icons (or use `Ctrl+space`).</li><li>Select an etity and from the dropdown menu, select Show helpers for selected entity.</li><li>Switch to hide helpers.</li></ol>								   |																																											  |
| Viewport Size															 | <ol><li>Select dropdown menu next to the double rectangle icon on the Viewport Toolbar > Ratio and switch between the ratios.</li><li>From the ratio menu select Custom... and input custom ratio values.</li><li>Select Resolution menu and switch between all of the resolutions in the menu.</li><li>Select Resolution > Custom... and input custom resolution values.</li></ol> |																						  |
| Grid/Angle snapping													 | <ol><li>Select dropdown menu next to the triple bar icon on the Viewport Toolbar and select Show Grid.</li><li>Select the Grid snapping option and change the Grid snapping size to 5.</li><li>Select an entity and using the translate mode, move it around.</li><li>Select the Angle Snapping option and change the Angle snapping size value to 60.</li><li>Select an entity and using the rotate mode, turn the entity around.</li></ol> | <ul><li>Grid displaying around the entity</li></ul>|
| Viewport  global preferences											 | <ol><li>Select Edit > Editor Settings > Global Preferences and select the Viewport tab.</li><li>Change FOV value and press `OK`.</li><li>Observe the changes in the Viewport.</li><li>Return to the Preferences menu, modify any other option, confirm and observe the changes.</li></ol>       | <ul><li>After restarting the Editor the aved changes should persist.</li></ul>																								  |
---

### Workflow 3: Entity manipulation 

**Product:** 
A Level with multiple entities with a mesh component and assigned shaderball model asset.
​
**Suggested Time Box:** 30 mins

| Workflow                                                               | Requests																																																																						   | Things to Watch For                                                                                                                                                          |
|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Manipulate the entity                                                  | <ol><li>Select an entity and use the Transform mode widgets to manipulate it.</li><li>Switch between translate, rotate and scale modes (`1`, `2` and `3` key accordingly).</li></ol>																												   | <ul><li>Shortcuts functionality.</li><li>Correct entity manipulation.</li></ul>																						  |
| Undo/Redo																 | <ol><li>While using the Transform mode widgets to manipulate an etity select Edit > Undo/Redo.</li><li>Manipulate the neity and use the shortcuts for Undo - `Ctrl+Z` and Redu `Ctrl+Shift+Z`.</li></ol>																							| <ul><li>Shortcuts functionality.</li></ul>																															      |
| Reference space widget                                                 | <ol><li>Select an entity and assign a child entity to it.</li><li>Select the child entity, move it around the Viewport and switch between the Reference spaces.</li></ol>																													   | <ul><li>Correct Translate and Rotate coordinates.</li><li>Correct entity manipulation.</li></ul>															     			  |
| Pivots																 | <ol><li>Select an entity with a three-dimensional shape.</li><li>Switch between pivots (`P` key) and manipulate the entity.</li></ol>																																							   | <ul><li>Schortcuts functionality.</li><li>Correct entity manipulation.</li></ul>																						  |
| Creating a custom reference spaces									 | <ol><li>Select an entity and select translation manipulator mode.</li><li>Press and hold `Ctrl+Alt` and select a target entity in the Viewport with `LMB`.</li><li>Use the manipulator to modify the entity.</li></ol>																		       | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																	  |
| Selecting a group of entities											 | <ol><li>Select multiple entities by using `LMB` and drag.</li><li>Deselect multiple entities by holding `Ctrl+LMB` and draging in the Viewport.</li><li>Add and remove an entity from the selection by holding `Ctrl` and `left-clicking` a target entity.</li></ol>					                   | <ul><li>Correct entity selection.</li></ul>																														  |
| Selecting a reference space for a group								 | <ol><li>With a group of entities selected, press and hold `Ctrl+Alt` and `left-click` a target entity.</li><li>Use the manipulator to modify the entities.</li></ol>																															       | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																	  |
| Creating a custom reference space for a group							 | <ol><li>With a group of entities selected, press and hold `Ctrl`, and move the manipulator in the Viewport.</li></ol>																																											   | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																	  |
| Parent reference spaces												 | <ol><li>Select multiple child entities of the same parent entity and use the manipulator to modify them.</li></ol>																																											   | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																		  |
| Using individual influence in a group								     | <ol><li>Press and hold `Alt` while modifying a group of selected entities to modify the entities in the local space.</li><li>Select child entities from different parent entities and press and hold `Alt`.</li><li>Press and release Alt to dynamically change the manipulator influence.</li></ol>| <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																	  |
| Reset an entity's transform, rotation, or scale						 | <ol><li>In the Viewport, select an entity or group of entities and press `R`.</li><li>Switch the manipulator mode and attempt to reset them.</li></ol>																																			   | <ul><li>Correct entity deafult location, rotation, and scale.</li></ul>																							      |
| Modifying manipulators												 | <ol><li>Select multiple entities.</li><li>Select the transform mode of the manipulator you want to modify.</li><li>Press and hold `Ctrl+LMB` and drag the manipulator to modify its location, rotation, or scale.</li></ol>														               | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																		  |
| Reset a manipulator's transform, rotation, or scale					 | <ol><li>Reset a manipulator to its default location, rotation, or scale by pressing `Ctrl+R` while the related manipulator is active.</li></ol>																																				   | <ul><li>Correct manipulator deafult location, rotation, and scale.</li></ul>																								  |
| Matching entity transforms											 | <ol><li>Select an entity.</li><li>Press and hold `Ctrl` and press the middle mouse button as you hover over a target entity. After the target is selected, the current selection transform matches that of the target.</li><li>Do this with location, rotation, or scale.</li></ol>			   | <ul><li>Correct matching location, rotation, and scale values.</li></ul>																									  |
| Ditto a group selection											     | <ol><li>Select a group of entities.</li><li>Press and hold `Ctrl` and press the `middle mouse button` as you hover over a target entity to match the group's transform data to the target's data.</li><li>Do this with location, rotation, or scale.</li></ol>									   | <ul><li>Correct matching location, rotation, and scale values.</li></ul>																								  |
| Ditto a group selection to local space								 | <ol><li>Select a group of entities.</li><li>Press and hold `Ctrl+Alt` and press the `middle mouse button` as you hover over a target entity. This sets the local space of each entity in the selected group to the target entity that you specified.</li></ol>								       | <ul><li>Correct matching location, rotation, and scale values.</li></ul>																								  | 
---
