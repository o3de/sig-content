*   [1 Introduction:](#TestPlan:ActionManager-1Introduction:)
    *   [1.1 References:](#TestPlan:ActionManager-1.1References:)
*   [2 Active Issues:](#TestPlan:ActionManager-2ActiveIssues:)
*   [3 Risks:](#TestPlan:ActionManager-3Risks:)
*   [4 Workflow Testing:](#TestPlan:ActionManager-4WorkflowTesting:)
*   [5 Automation:](#TestPlan:ActionManager-5Automation:)
    *   [5.1 Python Automation](#TestPlan:ActionManager-5.1PythonAutomation)
    *   [5.2 C++ Unit Tests](#TestPlan:ActionManager-5.2C++UnitTests)
*   [6 User-facing Documentation:](#TestPlan:ActionManager-6User-facingDocumentation:)

### **1 Introduction:**

This test plan is for validating the new Action Manager which improves Editor extensibility by allowing custom menus, hotkeys, and contextual actions to be registered via Gems/Python scripts.

##### **1.1 References:**

*   [Action Manager: Roadmap Task](https://github.com/o3de/o3de/issues/12380)
*   [Action Manager: RFC](https://github.com/o3de/sig-content/issues/51)


### **2 Active Issues:**

**NOTE:** This links to a project setup to capture all Action Manager issues  

[Action Manager Project Issues](https://github.com/orgs/o3de/projects/21)

### **3 Risks:**

| Risk | Mitigation                                                                                                                   | Detection Method |
| --- |------------------------------------------------------------------------------------------------------------------------------| --- |
| Existing Editor functionality is affected. | Enabling the new Action Manager will initially require a regset flag (regset=O3DE/ActionManager/EnableNewActionManager=true) | Existing automation and manual testing will be run before the flag is no longer required. |

### **4 Workflow Testing:**

[Editor Workflow Tests](https://github.com/o3de/sig-content/blob/main/testing-guidance/workflow-tests/editor/editor-workflow-tests.md#editor-workflow-tests) 1-3 should be run to validate a wide swath of toolbar, context menu, and hotkey functionality remains intact, with additional focus on Viewport interactions with gizmos and widgets.

### **5 Automation:**

All Editor automated tests should be run to similarly validate a wide swath of toolbar and context menu functionality remains intact.

  

##### **5.1 Python Automation**

Editor Python tests can be found in the Automated Testing project at AutomatedTesting/Gem/PythonTests/editor. These tests can be run via the steps below:

1.  Process all assets for the AutomatedTesting project.
2.  Run the following command from the o3de/python directory:

**Run Editor automated Python tests**

`python.cmd -m pytest AutomatedTesting/Gem/PythonTests/editor/TestSuite_Main.py --build-directory [cmake_build_dir]\bin\profile`

##### **5.2 C++ Unit Tests**

Editor C++ Unit tests can be found in AzToolsFramework.Tests.dll. These can be run via the steps below:

1.  AzToolsFramework.Tests.dll is built
2.  Run the following command from the desired build folder (e.g. profile, debug, etc.)

**Run Editor C++ Unit tests**

`[cmake_build_dir]\bin\[build_flavor]\AzTestRunner.exe AzToolsFramework.Tests.dll AzRunUnitTests --gtest_filter=*ActionManager*`

### **6 User-facing Documentation:**

**[https://www.o3de.org/docs/learning-guide/tutorials/extend-the-editor/](https://www.o3de.org/docs/learning-guide/tutorials/extend-the-editor/)**
