# Track View Workflow Tests

Testing in this area should focus on the functionality of Track View. This is a general tool for the creation, editing and exporting of cutscenes.

## General Docs

* https://www.o3de.org/docs/user-guide/visualization/cinematics/track-view/

###  Common Issues to Watch For

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.

* The Camera parameter does not take effect.
* Track View keyframe attributes cannot be modified.
* Nodes cannot be added to the Sequence.
* Unable to switch between multiple cameras.
* Editor or Track View crashes.
* Track View does not display the recorded Sequence.

## Workflows

### Area: Track View LKG Workflow

**Project Requirements**

* You've read through the Using the Track View Editor docs.
* FFMpeg plugin installed.

**Editor Platforms:**

* Windows
* Linux

**Game Launcher Supported Platforms:**

* Windows
* Linux

**Product:** Visible and responsive Track View editor.

**Suggested Time Box:** 15 minutes per platform.

| Workflow                                        | Requests           | Things to Watch For |
|-------------------------------------------------|--------------------|---------------------|
|Track View editor can be launched and configured| <ol><li>Open Track View editor by multiple ways including:<ul><li>Tools menu</li><li>Keyboard shortcut ( a shortcut can be assigned via Edit > Editor Settings > Keyboard Customization > Customize Keyboard)</li></ul></li><li>Perform resize, maximize, minimize undock, dock actions with Track View editor and its panes and drag them around the Editor.</li><li>Close and reopen the Track View editor.</li></ol> | <ul><li>Track View editor launch method does not work or does not launch quickly.</li><li>Track View editor not retaining configuration settings on close/relaunch.</li><li>Track View editor not retaining configuration settings on close/relaunch.</li><li>Track View panes not rendering, having rendering issues after being altered.</li><li>Not being able to resize, dock or drag panes borders.</li></ul>  |
|A Sequence can be created and configured                | <ol><li>Add Sequence to the Track View by multiple ways including:</li><ul><li>Tools menu</li><li>Node shortcut</li></ul><li>Name the Sequence.</li><li>Edit Sequence properties by multiple ways including:</li><ul><li>Enabling autostart</li><li>Editing Sequences name</li> <li>Changing start/end timings.</li> <li>Changing display time from seconds to frames.</li><li>Selecting different Out of Range options.</li></ul><li>Delete and recreate the Sequence.</li></ol> | <ul><li>The Sequence name is left empty.</li><li>Created Sequences do not appear in the Sequence dropdown.</li><li>Sequence properties changes do not take place in the Track Editor and the Curve Editor.</li></ul>  |
| Using basic Track View editor controls                  | <ol><li>Select the Shaderball entity in the viewport.</li><li>RMB on the Sequence and select "Add selected entity".</li><li>Add some keys to the position track.</li><li>Verify that Track View toolbars are enabled.</li></ol> | <ul><li>Entities do not add to the Sequence.</li><li>Keys cannot be edited with the Keys toolbar.</li></ul>  |
| FFmpeg plugin can be added to the Editor	              | <ol><li>You can open Start menu and find Registry Editor</li><li>In the Computer\HKEY_CURRENT_USER\SOFTWARE\O3DE\O3DE\, create Keys Settings</li><li>In the Settings, right click to add string value FFMPEG_PLUGIN and modify it to point to the path of ffmpeg executable</li><li>Reopen the Editor > Track View > Tools > Render Output.</li></ol> | <ul><li>FFMpeg plugin is not available.</li><li>After reopening, the Render Output tool shows errors or is not functional.</li></ul>  |
---

### Area: Default Track View tools

**Project Requirements**

* Track View editor opened with a working Sequence.
* Autostart is enabled for the Sequence.

**Product:**  Responsive Track View editor with full functionality.

**Suggested Time Box:**  60 minutes

| Workflow                     | Requests           | Things to Watch For |
|------------------------------|--------------------|---------------------|
| Configure nodes in the Sequence                         | <ol><li>RMB on the Node Browser and add all nodes from the list.</li><li>Rename nodes.</li><li>Copy/paste nodes.</li><li>Expand/Collapse all nodes.</li><li>Delete nodes.</li></ol> | <ul><li>Nodes cannot be added to the Sequence.</li><li>Nodes are disabled.</li><li>Nodes cannot be expanded and collapsed.</li><li>Nodes cannot be copied and pasted.</li><li>Nodes cannot be deleted.</li></ul>  |
| Configure entity nodes in the Track View	                         | <ol><li>Select Shaderball or create any other visible entity.</li><li>Enter the Track View.</li><li>RMB on the Node Browser and select "Add selected entity".</li><li>Enable Recording in the Play toolbar.</li><li>Move the entity in the Viewport to create a key in the Track editor.</li><li>In Track editor change position of the playhead.</li><li>Move the entity in the Viewport to create a second key in the Track editor.</li><li>Enter the Game Mode.</li></ol> | <ul><li>Some entities cannot be added to the Sequence.</li><li>Recording option cannot be enabled in the track editor's toolbar.</li><li>Keys are not added to the Track editor after the entity is moved in the Viewport.</li><li>Added keys have incorrect positions.</li><li>Key properties do not exist or cannot be edited.</li><li>The entity does not move in Game Mode.</li></ul>  |
| Configure Track editor toolbars	                         | <ol><li>Complete the steps for the "Configure entity nodes in the Track View".</li><li>Select all Track editors toolbar options.</li><li>Edit the keys added to the tracks.</li></ol> | <ul><li>Track editor toolbar options do not affect the keys.</li><li>The Sequence cannot be played, paused, resumed and stopped.</li><li>The Sequence cannot be played in a loop.</li><li>The framerate cannot be changed.</li><li>The snap options for the keys do not work.</li><li>The playhead cannot be moved with the toolbar options.</li></ul>  |
| Configure events in the Track View                         | <ol><li>RMB on the Sequence and select Edit Events.</li><li>Select the "Add" option and provide a valid event name.</li><li>In the Track editor add a key to the track.</li><li>In the key properties select the provided event name.</li><li>Open Script Canvas and create a simple script that triggers when the event is invoked.</li><li>Enter Game Mode.</li></ol> | <ul><li>Events cannot be created.</li><li>Key cannot be added to the tracker.</li><li>Events cannot be set in the keys properties.</li><li>Events are not triggered by the scripts.</li><li>Events are not triggered at the correct time.</li><li>Cannot create multiple events.</li><li>Events cannot be triggered one after another.</li></ul>  |
| Configure Curve editor	                         | <ol><li>Complete the steps for the "Configure entity nodes in the Track View".</li><li>Add 5 more keys to the tracker.</li><li>Select the component in the Node Browser.</li><li>Go to Curve editor.</li><li>Select any key in the Curve editor.</li><li>Move the selected key and observe changes on the Viewport.</li></ol> | <ul><li>Keys cannot be selected in the Curve editor by clicking or dragging.</li><li>Keys cannot be moved around the Curve editor.</li><li>Changes made to the keys are not visible in the Viewport.</li><li>Multiple keys cannot be moved at once.</li><li>Keys cannot be added/removed by double-clicking the line.</li></ul>  |
| Configure Curve editor toolbar	                         | <ol><li>Select one of the keys in the Curve editor.</li><li>Select each of the toolbar options, then move the key and the tangents.</li></ol> | <ul><li>The toolbar options cannot be selected.</li><li>Tangents and keys do not react differently after another toolbar option is selected.</li><li>Changes made in the Curve editor are not reflected in the running Sequence.</li></ul>  |
---
	
### Area: The Renderer Output tests

**Project Requirements**

* Track View editor opened with a working Sequence.
* Autostart is enabled for the Sequence.
* The FFmpeg plugin has been enabled. 

**Product:**  Responsive Renderer Output with full functionality.

**Suggested Time Box:**  60 minutes

| Workflow                     | Requests           | Things to Watch For |
|------------------------------|--------------------|---------------------|
| Basic functionality of the Renderer Output                         | <ol><li>Complete the steps for the "Configure entity nodes in the Track View".</li><li>Add Director node to the Sequence.</li><li>Open the Renderer Output.</li><li>Set destination folder for the output.</li><li>Add Batch for the Director node.</li><li>Start the output generation.</li></ol> | <ul><li>The Renderer Output tool does not open.</li><li>The destination folder cannot be set.</li><li>A batch Director cannot be added.</li><li>Frame files are not generated inside the selected directory.</li></ul>  |
| Configuration of the Renderer Output                         | <ol><li>Complete the steps for the "Basic functionality of the Renderer Output".</li><li>Change the Input settings.</li><li>Change the resolution outputs.</li><li>Change FPS options.</li><li>Change capture options.</li><li>Save Batch settings.</li><li>Change the Sequence and load the previous Batch.</li></ol> | <ul><li>Input settings cannot be changed.</li><li>Output settings cannot be changed.</li><li>An mp4 file cannot be created as an output.</li><li>Batch cannot be set, saved and loaded.</li></ul>  |
---

