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
| Navigate level with camera controls									 | <ol><li>Select the Viewport.</li><li>Navigate the camera with W, A, S, D, Q, E. Hold shift for fast move.</li><li>Right-click and drag to rotate the camera.</li><li>Press MMB and drag to pan up and down.</li><li>Scroll up and down using the MMB, to zoom in and out.</li><li>ALT+LMB to orbit.</li></ol>| <ul><li>Correct camera behaviour.</li></ul>																													  |
| Be this camera option													 | <ol><li>Select the camera entity or create one.</li><li>In the Entity Inspector, press the "Be this camera" button.</li><li>Move the camera around.</li><li>Press the Return to default editor camera button.</li></ol>																		   | <ul><li>Correct camera behaviour.</li></ul>																																  |
| Viewport camera selector												 | <ol><li>Open the Viewport Camera Selector (Tools > Viewport > Viewport Camera Selector).</li><li>Right-Click in the Viewport and select "Create camera entity from view".</li><li>Navigate to a different location in the Viewport and create another camera.</li><li>Navigate through the available cameras in the Viewport Camera Selector while monitoring the Viewport.</li></ol>| <ul><li>Cameras added to the level.</li><li>Correct camera switching and behaviour.</li></ul>|
---

### Workflow 2: Viewport options  
​
**Product:** 
A Level with multiple entities with a mesh component and assigned shaderball model asset.
​
**Suggested Time Box:** 10 mins

| Workflow                                                               | Requests																																																																						   | Things to Watch For                                                                                                                                                          |
|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hide/Unhide															 | <ol><li>Select an entity with mesh component in the Entity Outliner and press the eye icon.</li><li>Select the Show All menu option (Edit > Show All).</li><li>Select the entity in the Viewport.</li><li>Press "H" to hide the entity.</li><li>Press "Ctrl+SHIFT+H" to unhide the entity.</li></ol>|<ul><li>Correct entity Hide/Unhide functionality.</li><li>Shortcut functionality.</li></ul>															                      |
| Lock/Unlock															 | <ol><li>Select an entity with mesh component in the Entity Outliner, press the lock icon.</li><li>Attempt to select the entity in the Viewport.</li><li>Select the Unlock All menu option (Edit > Unlock All).</li><li>Select the entity in the Viewport and press "L" to lock the entity.</li><li>Press "Ctrl+SHIFT+L" to unlock all entities.</li></ol>|<ul><li>Correct entity Lock/Unlock functionality.</li><li>Shortcut functionality.</li></ul>						  |
| Go to position														 | <ol><li>Select View > Viewport > Go to Position...</li><li>Input coordinates.</li></ol>																																																		   | <ul><li>Correct camera behaviour.</li></ul>																																  |
| Viewport  global preferences											 | <ol><li>Select Edit > Editor Settings > Global Preferences and select the Viewport tab.</li><li>Change FOV value and press OK.</li><li>Observe the changes in the Viewport.</li><li>Return to the Preferences menu, modify any other option, confirm and observe the changes.</li></ol>         | <ul><li>Correct Viewport behaviour.</li></ul>																																  |
---

### Workflow 3: Entity manipulation 

**Product:** 
A Level with multiple entities with a mesh component and assigned shaderball model asset.
​
**Suggested Time Box:** 30 mins

| Workflow                                                               | Requests																																																																						   | Things to Watch For                                                                                                                                                          |
|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Manipulate the entity                                                  | <ol><li>Select an entity and use the Transform mode widgets to manipulate it.</li><li>Switch between translate, rotate and scale modes (1, 2 and 3 key accordingly).</li></ol>																												   | <ul><li>Shortcuts functionality.</li><li>Correct entity manipulation.</li></ul>																						      |
| Reference space widget                                                 | <ol><li>Select an entity and assign a child entity to it.</li><li>Select the child entity, move it around the Viewport and switch between the Reference spaces.</li></ol>																													   | <ul><li>Correct Translate and Rotate coordinates.</li><li>Correct entity manipulation.</li></ul>															     			  |
| Pivots																 | <ol><li>Select an entity with a three-dimensional shape.</li><li>Switch between pivots (P key) and manipulate the entity.</li></ol>																																							   | <ul><li>Schortcuts functionality.</li><li>Correct entity manipulation.</li></ul>																							  |
| Creating a custom reference spaces									 | <ol><li>Select an entity and select translation manipulator mode.</li><li>Press and hold Ctrl+Alt and select a target entity in the Viewport with LMB.</li><li>Use the manipulator to modify the entity.</li></ol>																		       | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																		  |
| Selecting a group of entities											 | <ol><li>Select multiple entities by using LMB and drag.</li><li>Deselect multiple entities by holding Ctrl+LMB and draging in the Viewport.</li><li>Add and remove an entity from the selection by holding Ctrl and left-clicking a target entity.</li></ol>					                   | <ul><li>Correct entity selection.</li></ul>																																  |
| Selecting a reference space for a group								 | <ol><li>With a group of entities selected, press and hold Ctrl+Alt and left-click a target entity.</li><li>Use the manipulator to modify the entities.</li></ol>																															       | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																	      |
| Creating a custom reference space for a group							 | <ol><li>With a group of entities selected, press and hold Ctrl, and move the manipulator in the Viewport.</li></ol>																																											   | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																		  |
| Parent reference spaces												 | <ol><li>Select multiple child entities of the same parent entity and use the manipulator to modify them.</li></ol>																																											   | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																		  |
| Using individual influence in a group								     | <ol><li>Press and hold Alt while modifying a group of selected entities to modify the entities in the local space.</li><li>Select child entities from different parent entities and press and hold Alt.</li><li>Press and release Alt to dynamically change the manipulator influence.</li></ol>| <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																	      |
| Reset an entity's transform, rotation, or scale						 | <ol><li>In the Viewport, select an entity or group of entities and press R.</li><li>Switch the manipulator mode and attempt to reset them.</li></ol>																																			   | <ul><li>Correct entity deafult location, rotation, and scale.</li></ul>																									  |
| Modifying manipulators												 | <ol><li>Select multiple entities.</li><li>Select the transform mode of the manipulator you want to modify.</li><li>Press and hold Ctrl+LMB and drag the manipulator to modify its location, rotation, or scale.</li></ol>														               | <ul><li>Correct manipulator match to the translation or orientation of the target entity.</li></ul>																		  |
| Reset a manipulator's transform, rotation, or scale					 | <ol><li>Reset a manipulator to its default location, rotation, or scale by pressing Ctrl+R while the related manipulator is active.</li></ol>																																				   | <ul><li>Correct manipulator deafult location, rotation, and scale.</li></ul>																								  |
| Matching entity transforms											 | <ol><li>Select an entity.</li><li>Press and hold Ctrl and press the middle mouse button as you hover over a target entity. After the target is selected, the current selection transform matches that of the target.</li><li>Do this with location, rotation, or scale.</li></ol>			   | <ul><li>Correct matching location, rotation, and scale values.</li></ul>																									  |
| Ditto a group selection											     | <ol><li>Select a group of entities.</li><li>Press and hold Ctrl and press the middle mouse button as you hover over a target entity to match the group's transform data to the target's data.</li><li>Do this with location, rotation, or scale.</li></ol>									   | <ul><li>Correct matching location, rotation, and scale values.</li></ul>																									  |
| Ditto a group selection to local space								 | <ol><li>Select a group of entities.</li><li>Press and hold Ctrl+Alt and press the middle mouse button as you hover over a target entity. This sets the local space of each entity in the selected group to the target entity that you specified.</li></ol>								       | <ul><li>Correct matching location, rotation, and scale values.</li></ul>																									  | 
---