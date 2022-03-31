### Summary:
GridMate and GridHub are Lumberyard's prior solutions for Networking, Multiplayer and Discoverability. They are largely unused, unmaintained and undocumented. They have functionally been replaced by AzNetworking for Networking and Transport and the Multiplayer Gem for Multiplayer.

Currently the only dependency on GridMate and GridHub is the Lua Editor. The Lua Editor should be migrated away from GridHub using the mechanism proposed in this RFC. Once the Lua editor is migrated from GridHub, all of Gridmate and GridHub can be safely removed from the engine. The changes involved in this migration should keep the Lua Editor functionally the same though there will be some minor changes as detailed in the Technical Design.

### What is the relevance of this feature?
This work migrates the Lua Editor, a highly used feature, from using deprecated libraries to O3DEs current transport solution. The Lua Editor is the last user of these deprecated libraries so this will enable the removal of unmaintained libraries from O3DE. This reduces the code footprint of the engine in addition to confusion by leaving AzNetworking and Multiplayer as the de factor transport and multiplayer libraries.

### Feature design description:
The Lua Editor provides the ability to edit and debug lua scripts. The debugging utilities allows a user to connect to an O3DE application running Lua scripts to enable step through and breakpoint style debugging.

In order to identify which applications can be connected to, the Lua Editor uses GridHub to discover relevant applications. The Lua Editor can then list these applications in a drop down UI menu which allows users to select which application they want to attach to for debugging purposes. Given this is the only usage of discoverability in the engine remaining, the proposal is to change how the Lua Editor connection is established while maintaining the existing user experience.

The Lua Editor will act as a server/listener for connections. Applications that would normally be discovered by GridHub will poll a connection periodically to a prospective host (Lua Editor in this case). The connection will be established via TCP over a user specified port. As part of AzNetworking, users can enable TLS for the TCP connection if desired. Once connected, the user experience will remain the same. Users can select a debug target from a dropdown in the Lua Editor.

Once this migration is completed, GridHub/GridMate and all remaining references to it can be safely removed.

### Technical design description:
The current GridHub based algorithm looks something like this:

1. LuaIDE launches GridHub on Windows. GridHub discovers available targets and makes it known to LuaIDE

2. On target select, a GridMate connection is established over local host between the target and LuaIDE

3. GridMate communicates init data using a Replica (one of its transport communication mechanisms)

4. The TargetManagement system in AzFramework maintains an inbox and outbox of messages that it sends as marshalled buffers over GridMate.

In order to replace these we'll do the following:

1. LuaIDE launches on all platforms as a localhost listener on a designated port.

2. Each valid target launches on all platform and will poll a connection to a target host (LuaIDE in this case) until a connection is established.

3. An AzNetworking packet is sent to transmit init data from the target application (such as Editor) to the Lua IDE.

4. On receipt, the target application becomes a valid target of the host and can be selected from the Debugging dropdown.

5. The TargetManagement system in AzFramework maintains an inbox and outbox of messages that it sends as marshalled buffers over AzNetworking via packets.

As part of this migration the following changes occur:

1. TargetManagement Connections can no longer be established in RELEASE builds
2. Connections can be established on all platforms. Previously only Windows was supported.
3. Connection port can be configured by CVar however all connections will remain on localhost
4. Connections can be TLS encrypted
Given the above the attack surface is expected to decrease.

We'll be moving from having GridHub discover targets for the LuaIDE and mediate a socket based GridMate connection to the target to the target establishing a direct socket connection with the Lua IDE.
The data being sent does not change.

### How will this be implemented or integrated into the O3DE environment?
- AzFramework will add AzNetworking as a dependency effectively replacing GridMate/GridHub.

- AzNetworking transport and packets will be used to replace the connection and communication mechanisms currently implemented through GridMate and GridHub in AzFramework's Target Management System.

### Are there any alternatives to this feature? 

- We could implement a new discoverability feature in AzNetworking however given Lua Editor is the only usage of GridHub discoverability, this seemed excessive. 
- We could replace Lua Editor with VS Code however this would expand the scope and should be considered as a separate feature.

### How will users learn this feature?
No learning will be required on behalf of the user. These changes are all obfuscated from users by virtue of keeping the user experience for the Lua Editor exactly the same. Relevant documentation will be updated to remove references to GridMate/GridHub.

### Are there any open questions?
N/A