# Lua Scripting Workflow Tests

Testing in this area should focus on the functionality of Lua Editor.

## Common Issues to Watch For

Test guidance will sometimes note specific issues to watch for. The common issues below should be watched for through all testing, even if unrelated to the current workflow being tested.
1. Asset processor errors when loading/saving .lua files
2. Warnings or assets that appear in the Log
3. Unresponsive UI elements 

### Platforms:

- Windows
- Linux

### Docs:

* [O3DE Documentation: Writing Lua Scripts in Open 3D Engine](https://www.o3de.org/docs/user-guide/scripting/lua/)
* [O3DE Documentation: Introduction to Open 3D Engine’s Lua Editor](https://www.o3de.org/docs/user-guide/scripting/lua/lua-editor/)
* [O3DE Documentation: Basic Structure of a Lua Script](https://www.o3de.org/docs/user-guide/scripting/lua/basic-lua-script/)
* [O3DE Documentation: Debugging Lua Scripts](https://www.o3de.org/docs/user-guide/scripting/lua/debugging-scripts/)
* [O3DE Documentation: Debugging with Lua Editor](https://www.o3de.org/docs/user-guide/scripting/lua/debugging-tutorial/)
* [O3DE Documentation: Using EBuses in Lua](https://www.o3de.org/docs/user-guide/scripting/lua/ebus/)

### Script Files:

* ["Area: Creating a Lua skeleton script file" BaseScript.lua](#script_0)
* ["Area: Properties usage in Lua scripts" BaseScript.lua](#script_1)
* ["Area: Moving entity using Lua script" BaseScript.lua](#script_2)
* ["Area: Using ScriptEvents to communicate between two Lua scripts" SendEvent.lua and ReceiveEvent.lua](#script_3)

## Area: Opening Lua Editor, connecting to Editor and using Find tool

### Project Requirements

* Any project with the Remote Tools Connection Gem

**Product:** 

Running Lua Editor with basic functions checked.

**Suggested Time Box:** 

20 minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| Launch Lua Editor and connect to Editor |<ol><li> Check different methods of entry to Lua Editor: <ul><li>*Tools → Lua Editor*</li><li>Lua Script component</li></ul></li> <li>Modify Lua Editor layout:<ul><li>Resize the Lua Editor window</li><li>Undock panes</li><li>Move panes around</li><li>Dock panes</li></ul></li><li> Connect to Editor by clicking *Target* button and choosing *Editor.exe (xyzxyzxy)* from the dropdown ![Lua Editor Target Button](images/Luaeditor_target_button.png)</li></ol>|<ul><li>Lua Editor launches and is stable </li><li> Window and panes can be moved, resized and properly docked/undocked </li> <li>Lua Editor can be connected to Editor</li> <li>Class Reference is populated after connecting to Editor</li> </ul>|
| Create a new .lua file and check Log messages |<ol><li>Create a new .lua file by selecting *File → New* in Lua Editor, make sure to save it in a path accessible to Editor (for example *<project path\>\\Assets\\*)</li><li>Type few random characters into the script and save it using: <ul><li>*File → Save*</li><li>*Ctrl+S* shortcut</li><li>*File → Save As* in a separate file</li></ul></li><li>Reopen the original .lua file after performing *File → Save As* operation</li><li>Create one more new .lua file same way as in step 1 and move the script tabs between each other</li><li>Add a change to each opened script (so that * is visible on their tabs) and use *File → Save All*</li></ol>|<ul><li>New file can be created and is opened in Lua Editor</li><li>With multiple .lua files opened, their tabs can be rearranged freely</li><li>Files can be saved using different available options</li><li>Log produces error messages when saving .lua files with no valid script structure (random characters in this case)</li></ul>|
| Basic Find Tool usage | <ol><li>Add a unique string to any of the opened scripts (for example *text*)</li><li>Check different methods of opening *Find* tool:<ul><li>*Edit → Find*</li><li>Toolbar magnifying glass button ![Lua Editor Find Button](images/Luaeditor_find_button.png)</li><li>*Ctrl+F* shortcut</li></ul></li><li>Type the unique string into *Text to find:* textbox and click *Find Next* button</li><li>Add the same unique string to two other opened files</li><li>In *Find* tool change *Current File* dropdown to *All Open Files* and click *Find All* button</li><li>Type a new unique string into *Replace With:* textbox (for example *Hello, World!*) and click *Replace All* button</li></ol>|<ul><li>*Find* tool opens and is stable</li><li>Text search highlights matching text in individual and multiple files</li><li>Text can be replaced from *Find* tool's window</li><ul>|

## Area: Creating a Lua skeleton script file 

### Project Requirements

* Any project

**Assets:**

* ["Area: Creating a Lua skeleton script file" BaseScript.lua](#script_0)

**Product:** 

Skeleton Lua script assigned to an entity and printing simple messages to console when entering/exiting Game Mode.

**Suggested Time Box:** 

20 minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| Create a Lua skeleton script | <ol><li>Create a new .lua file by selecting *File → New* in Lua Editor, make sure to save it in a path accessible to Editor (for example *<project path\>\\Assets\\*)</li><li>Add a [skeleton Lua script](https://www.o3de.org/docs/user-guide/scripting/lua/basic-lua-script/#skeleton-script) to the file - remember to replace *BaseScript* in the file to the name of your lua file if it is different (you can do this easily using *Find* tool and *Replace All* action)</li><li>Replace `-- Activation Code` and `-- Deactivation Code` lines with `Debug.Log(tostring("Hi!"));` and `Debug.Log(tostring("Bye!"));` respectively</li><li>Save the .lua file (for example *File → Save* or *Ctrl+S*)</li></ol>| <ul><li>Basic script can be created and saved</li><li>After saving *Compilation Succesful - <path\>* is printed to Log and no errors occur</li></ul>|
| Create an entity using the created skeleton Lua script | <ol><li>In Editor, create a new entity and add a *Lua Script* component to it</li><li>Select *Lua Script* component on your new entity, click the *Pick Lua Script* button ![Lua Component Pick Lua Script Button](images/Luacomponent_pick_lua_script_button.png) and choose your skeleton script</li><li>Enter and exit Game Mode (using for example *Ctrl+G*) while paying attention to the Editor's Console</li></ol> | <ul><li>*Lua Script* component can be assigned to an entity</li><li>Lua script can be assigned to *Lua Script* component</li><li>*Hi!* is printed to Console on entering the Game Mode and *Bye!* on exiting it</li><li>Editor remains stable</li><li>Final working script can be found below this table</li></ul> |

<details><summary><b>(Click to Expand) BaseScript.lua:</b><a name="script_0"></a></summary>
<p>

```
-- BaseScript.lua 

local BaseScript = 
{
    Properties =
    {
        -- Property definitions
    }
}

function BaseScript:OnActivate()
     Debug.Log(tostring("Hi!"));
end

function BaseScript:OnDeactivate()
     Debug.Log(tostring("Bye!"));
end

return BaseScript
```
</p>
</details>

## Area: Properties usage in Lua scripts

### Project Requirements

* Any project can be used
* Saved script from "Area: Lua skeleton script usage" helps with setup

**Assets:**

* ["Area: Properties usage in Lua scripts" BaseScript.lua](#script_1)

**Product:** 

Lua Script returning messages to Editor Console using properties declared in the script.

**Suggested Time Box:**

20 minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| Add a property to Properties table in a script | <ol><li>Add a property to the `Properties` table of opened Lua script (example of this can be viewed under `local BaseScript = {}` table in the **(Click to Expand) BaseScript.lua** below)</li><li>Add a line to OnActivate() that prints the property after a string (for example change `Debug.Log(tostring("Hi!"));` to `Debug.Log(tostring("Hi! My value is: " .. BaseScript.Properties.MyValue));` - where BaseScript is the name of your Lua script)</li><li>Save the file and run Game Mode with this script added to a Lua Script entity</li></ol> | <ul><li>File is saved without error</li><li>When entering Game Mode, the property is properly printed (for example *Hi! My value is: 1*)</li></ul> |
| Add local properties in a script | <ol><li>Write a code which declares two properties, adds them together in a third property and prints that third property (example of this can be viewed under `OnDeactivate()` function in the **(Click to Expand) BaseScript.lua** below) </li><li>Modify the Debug.Log line so that it uses the local properties - for example: `Debug.Log(tostring(Bye .. NumberOne .. " + " .. NumberTwo .. " = " .. NumbersAdded));`</li><li>Save the file and run and exit Game Mode with this script added to a Lua Script entity</li></ol> | <ul><li>File is saved without error</li><li>When exiting Game Mode, the properties are properly printed (for example *Bye! 1 + 2 = 3*)</li></ul> |

<details><summary><b>(Click to Expand) BaseScript.lua:</b><a name="script_1"></a></summary>
<p>

```
-- BaseScript.lua 

local BaseScript = 
{
    Properties =
    {
        MyValue = 1,
    }
}

function BaseScript:OnActivate()
     Debug.Log(tostring("Hi! My value is: " .. BaseScript.Properties.MyValue));
end

function BaseScript:OnDeactivate()
     Bye = "Bye! "
     NumberOne = 1
     NumberTwo = 2
     NumbersAdded = NumberOne + NumberTwo
     Debug.Log(tostring(Bye .. NumberOne .. " + " .. NumberTwo .. " = " .. NumbersAdded));
end

return BaseScript
```
</p>
</details>

## Area: Debugging existing script

### Project Requirements

* Any project with the Remote Tools Connection Gem added
* Lua Editor connected to Editor
* Saved BaseScript.lua from "Area: Properties usage in Lua scripts" helps with setup

**Product:** 

Simple debugging setup created and confirmed to be working.

**Suggested Time Box:**

20 minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| Open Debugging tools | <ol><li>Open Breakpoints tool using any method: <ul><li>Toolbar Breakpoints button ![Lua Editor Breakpoints Button](images/Luaeditor_breakpoints_button.png)</li><li>*View → Breakpoints*</li></ul></li><li>Add breakpoints to some lines on your script using any method: <ul><li>Select a line and click/press: <ul><li>Toggle Breakpoint button on toolbar ![Lua Editor Toggle Breakpoint Button](images/Luaeditor_toggle_breakpoint_button.png)</li><li>*Debug → Toggle Breakpoint*</li><li>*F9* key</li></ul> <li>Left-click the area next to the line number on the script</li></ul></li><li>Remove added breakpoints (This can be done using the same methods as adding them)</li><li>Open Stack tool using any method: <ul><li>Toolbar Stack button ![Lua Editor Stack Button](images/Luaeditor_stack_button.png)</li><li>*View → Stack*</li></ul> </li><li>Open LUA Locals tool using any method:<ul><li>Toolbar LUA Locals button ![Lua Editor LUA Locals Button](images/Luaeditor_lua_locals_button.png)</li><li>*View → LUA Locals*</li></ul></li></ol> | <ul><li>Breakpoints tool can be opened and tracks placed breakpoints</li><li>Breakpoints can be removed</li><li>Stack tool can be opened</li><li>LUA Locals tool can be opened</li></ul> |
| Basic breakpoint use | <ol><li>Add a breakpoint to a `Debug.Log();` line in `OnActivate()` function (for example line 12 in the "Area: Properties usage in Lua scripts" script) </li><li>Make sure that Lua Editor is connected to the Editor and enter Game Mode</li><li>Change the window to the Lua Editor while the Game Mode is running (for example using Alt+Tab navigate to the Lua Editor window)</li><li>Continue with script execution using any of the available options until the Game Mode is functional: <ul><li>Run/Continue:<ul><li>Toolbar Run/Continue button ![Lua Editor Run/Continue Button](images/Luaeditor_run_continue_button.png)</li><li>*Debug → Run/Continue*</li><li>*F5* key</li></ul></li><li>Step Over:<ul><li>Toolbar Step Over button ![Lua Editor Step Over Button](images/Luaeditor_step_over_button.png)</li><li>*Debug → Step Over*</li><li>*F10* key</li></ul></li><li>Step Into:<ul><li>Toolbar Step In button ![Lua Editor Step In Button](images/Luaeditor_step_in_button.png)</li><li>*Debug → Step Into*</li><li>*F11* key</li></ul></li><li>Step Out:<ul><li>Toolbar Step Out button ![Lua Editor Step Out Button](images/Luaeditor_step_out_button.png)</li><li>*Debug → Step Out*</li><li> *Shift+F11* shortcut</li></ul></li></ul></li></ol> | <ul><li>Stack and LUA Locals populate with data when entering Game Mode</li><li>Script execution stops on the breakpoint (Current executed line is marked by a yellow triangle pointing to the right, next to the line number)</li><li>Continuing with script execution updates the yellow triangle position until it leaves the `OnActivate()` function</li></ul> |

## Area: Moving entity using Lua script 

### Project Requirements

* Any project with the Remote Tools Connection Gem
* Lua Editor connected to Editor
* Saved BaseScript.lua from "Area: Properties usage in Lua scripts" helps with setup

**Assets:**

* ["Area: Moving entity using Lua script" BaseScript.lua](#script_2)

**Product:** 

Entity that can be moved with keyboard input in Game Mode using Lua Script.

**Suggested Time Box:**

30 minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| Create .inputbidings file | <ol><li>Open Asset Editor (for example *Tools → Asset Editor)* and create a new .inputbindings file (*File → New → Input Bindings*)</li><li>Add three following Input Event Groups to the .inputbindings file: <ul><li>Event Name: `Forward`; Event Generators: `keyboard_key_alphanumeric_W` with Event value multiplier: `1.0` and `keyboard_key_alphanumeric_S` with Event value multiplier: `-1.0`</li><li>Event Name: `Strafe`; Event Generators: `keyboard_key_alphanumeric_A` with Event value multiplier: `-1.0` and `keyboard_key_alphanumeric_D` with Event value multiplier: `1.0`</li><li>Event Name: `Look`; Event Generators: `mouse_delta_x` with Event value multiplier: `-1.0`</li></ul></li><li>Save the .inputbindings file for example in the project's Assets folder under the name *PlayerMovement*</li></ol> | <ul><li>New .inputbinding file can be created</li><li>Event Groups can be added to Input Bindings and can be modified</li><li>Input Bindings can be saved</li></ul> |
| Modify the Lua script so that it uses keyboard input to move the entity | <ol><li>Modify the script as shown below the table in under the **(Click to Expand) BaseScript.lua**</li><li>Find the following copied functions in the Class Reference (Remember that Lua Editor has to be connected to the Editor):<ul><li>GetEntityForward</li><li>AddVelocity</li><li>GetEntityRight</li><li>GetTickDeltaTime</li><li>RotateAroundLocalZ</li></ul></li><li>Save the modified Lua script</li></ol> | <ul><li>Referenced functions can be found in Class Reference</li><li>Lua script can be saved without errors</li></ul> |
| Create and move Player entity in Game Mode | <ol><li>Create an entity with the following components: <ul><li>*Mesh*</li><li>*Input*</li><li>*PhysX Character Controller*</li><li>*PhysX Character Gameplay*</li></ul></li><li>Assign the PlayerMovement.inputbindings to the Input component</li><li>Assign the Lua script to the Lua Script component.</li><li>Add a Model Asset to the Mesh component (for example BeveledCylinder.fbx)</li><li>Add a child *Camera* entity to the Player entity</li><li>Add a Camera component to the *Camera* child entity and set the Transform's Translate to `{X = 0.0; Y = -3.0; Z = 2.0}` </li><li>Enter the Game Mode and move the entity using mouse and WASD keys </li></ol> | <ul><li>Mentioned components can be added to entities and relevant files can be assigned to their respective components</li><li>Child entity can be added to an entity and it's Transform adjusts based on the parent entity</li><li>Player entity can be moved and rotated around in the Game Mode using defined input keys/mouse</li></ul> |

<details><summary><b>(Click to Expand) BaseScript.lua:</b><a name="script_2"></a></summary>
<p>

```
-- BaseScript.lua 

local BaseScript = 
{
    Properties =
    {
        MyValue = 1,
    }
}

function BaseScript:OnActivate()
	--Bind Input Event Groups from PlayerMovement.inputbindings
	--Forward
	self.forwardBusId = InputEventNotificationId("Forward");
	self.forwardBus = InputEventNotificationBus.Connect(self, self.forwardBusId);
	--Strafe
	self.strafeBusId = InputEventNotificationId("Strafe");
	self.strafeBus = InputEventNotificationBus.Connect(self, self.strafeBusId);
	--Look
	self.lookBusId = InputEventNotificationId("Look");
	self.lookBus = InputEventNotificationBus.Connect(self, self.lookBusId);
end

--OnHeld is activated when an input is being held or applied constantly (as in the case of mouse movement)
function BaseScript:OnHeld(inputValue)
	--Create a reference to the currently used input
	local currentBusId = InputEventNotificationBus.GetCurrentBusId();
	
	--If "Forward" input is held (current used input is equal to "Forward") then
	--Create a vector variable based on the entity's forward vector(EntityEntity_VM.GetEntityForward) with length equal to value of the input(inputValue)
	--Add velocity (CharacterControllerRequestBus.Event.AddVelocity) equal to the above vector (self.forwardVector) to the entity housing this script (self.entityId)
	if currentBusId == self.forwardBusId then
		self.forwardVector = EntityEntity_VM.GetEntityForward(self.entityId, inputValue);
		CharacterControllerRequestBus.Event.AddVelocity(self.entityId, self.forwardVector);
	end
	
	--If "Strafe" input is held (current used input is equal to "Strafe") then
	--Create a vector variable based on the entity's right vector(EntityEntity_VM.GetEntityRight) with length equal to value of the input(inputValue)
	--Add velocity (CharacterControllerRequestBus.Event.AddVelocity) equal to the above vector (self.rightVector) to the entity housing this script (self.entityId)
	if currentBusId == self.strafeBusId then
		self.rightVector = EntityEntity_VM.GetEntityRight(self.entityId, inputValue);
		CharacterControllerRequestBus.Event.AddVelocity(self.entityId, self.rightVector);
	end
	
	--If "Look" input (horizontal mouse movement) is used (current used input is equal to "Forward") then
	--Create a float variable that is a multiplication of the input value (inputValue) and time elapsed from the last tick (TickRequestBus.Broadcast.GetTickDeltaTime())
	--Apply rotation around Z axis of entity housing this script (TransformBus.Event.RotateAroundLocalZ(self.entityId,...) equal to the above float (self.rotateSpeed)
	if currentBusId == self.lookBusId then
		self.rotateSpeed = inputValue * TickRequestBus.Broadcast.GetTickDeltaTime();
		TransformBus.Event.RotateAroundLocalZ(self.entityId, self.rotateSpeed);
	end
end

function BaseScript:OnDeactivate()
     Bye = "Bye! "
     NumberOne = 1
     NumberTwo = 2
     NumbersAdded = NumberOne + NumberTwo
     Debug.Log(tostring(Bye .. NumberOne .. " + " .. NumberTwo .. " = " .. NumbersAdded));
end

return BaseScript
```
</p>
</details>

## Area: Using ScriptEvents to communicate between two Lua scripts

### Project Requirements

* Any project

**Assets:**

* ["Area: Using ScriptEvents to communicate between two Lua scripts" SendEvent.lua and ReceiveEvent.lua](#script_3)

**Product:** 

Two Lua scripts, where one of the scripts uses variable passed to it from the other script.

**Suggested Time Box:**

20 minutes

| Workflow | Requests | Things to Watch For |
|----------|----------|---------------------|
| Create a Script Event | <ol><li>Open Asset Editor (for example *Tools → Asset Editor)* and create a new .scriptevents file (*File → New → Script Events*)</li><li>Modify the created Script Events file: <ul><li>Name: SimpleScriptEvent</li><li>Add an Event to the Events</li><li>Event Name: SendNumber</li><li>Add a Parameter to the Parameter of SendNumber event</li><li>Parameter Name: Number</li><li>Parameter Type: Number</li></ul></li><li>Save the .scriptevents file for example in the project's Assets folder under the name *SimpleScriptEvent* </li></ol> | <ul><li>New Script Events file can be created</li><li>Script Events file can be modified</li><li>Script Events file can be saved</li></ul> |
| Create Lua scripts utilizing the Script Event to send a variable from one to another | <ol><li>Create two new Lua scripts and copy the contents of *SendEvent.lua* and *ReceiveEvent.lua* found under the **(Click to Expand) SendEvent.lua and ReceiveEvent.lua:** to them</li><li>Save the Lua scripts with their respective names: *SendEvent.lua* and *ReceiveEvent.lua*</li></ol> | <ul><li>New Lua scripts can be created</li><li>Lua scripts save successfully with provided code</li></ul> |
| Create entities using the Lua scripts and enter Game Mode | <ol><li>Create two entities with Lua Script component added to them</li><li>Assign *SendEvent.lua* to one of the entities and *ReceiveEvent.lua* to the other</li><li>Enter Game Mode</li></ol> | <ul><li>Entities with Lua Script components can be created</li><li>Lua scripts can be assigned to the Lua Script component</li><li>Assigned scripts execute printing *Received number: 123.0* about every second</li></ul> |

<details><summary><b>(Click to Expand) SendEvent.lua and ReceiveEvent.lua:</b><a name="script_3"></a></summary>
<p>

*SendEvent.lua*
```
local SendEvent = 
{
}

function SendEvent:OnActivate()
	--Connect to Tick bus, so that OnTick(dt) function is available further on
	self.tickHandler = TickBus.Connect(self)
	--Create a timer variable with value of 1, later used in the delay setup
	timer = 1
	--Connect to the created Script Event (SimpleScriptEvent is the name of the Script Event file)
	self.ScriptEventHandler = SimpleScriptEvent.Connect(self)
end

--The delay setup is put in place so that the Send event is guaranteed to launch after all other entities/scripts (including the Receive event) are active

--OnTick is activated each tick returning delta time (dt) since the last tick
function SendEvent:OnTick(dt)
	--Each tick subtract the time it took since the last tick from the current timer value and overwrite it (Can be looked at as "Count from 1 to 0 using ticks")
	timer = timer - dt
	--If timer variable reaches 0 or negative values then
	if timer <= 0 then
		--Send the SimpleScriptEvent's SendNumber event with a Number parameter 123
		SimpleScriptEvent.Broadcast.SendNumber(123)
		--Reset the timer variable to 1
		timer = 1
	end
end

return SendEvent
```
*ReceiveEvent.lua*
```
local ReceiveEvent = 
{
}

function ReceiveEvent:OnActivate()
	--Connect to the created Script Event (SimpleScriptEvent is the name of the Script Event file)
	self.ScriptEventHandler = SimpleScriptEvent.Connect(self, self.entityId)

end

--Receive the SimpleScriptEvent's SendNumber event with it's parameter (number)
function ReceiveEvent:SendNumber(number)
	--Print the received parameter to the Console
	Debug.Log("Received number: " .. tostring(number))
end

return ReceiveEvent
```
</p>
</details>
