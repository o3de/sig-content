## Overview

This feature proposes a View Bookmark System in which the user can store several camera properties (position, orientation, fov, etc.) and allow them to group these Bookmarks in order to improve workflows such as level or environment design.

### What is the relevance of this feature?

Currently, the only feature that can accomplish a similar behavior is the "Viewport Camera Selector". Nevertheless, this feature was not designed for that use and has the following drawbacks:

1. It cycles between entities with camera components, so every time we want to add a new camera we need to create an entity with a camera attached to it. These entities are game objects that, if they are not removed, would be shipped to the final product.
2. The cameras cannot be grouped.
3. You can only modify the camera properties by accessing the entity that holds the camera component and then changing the properties in the component itself.

With the new system we would be storing just Bookmarks which are structs that contain different properties for the camera and then, when the user accesses one of those Bookmarks, we would load these properties into the main viewport camera. These Bookmarks also hold other information such as tags that will helpful in organizing widgets to easily display these Bookmarks.

### Feature design description

This feature is all based in the View Bookmark concept which is a struct in which we store camera properties. Depending on the use case we want, these Bookmarks could be kept in memory or stored persistently. If they are stored persistently we may want to have them stored locally or shared amongst the development team. For Example:

| **Item**                                                                                               | **Stored/Not stored** | **Local/Shared** |
| ------------------------------------------------------------------------------------------------------ | --------------------- | ---------------- |
| Restore the camera transform to the last known location of the camera in the level when it is opened.  | **Stored**            | **Local**        |
| Transient Camera Transform (used to remember the camera transform when going in and out of game mode.) | **Not Stored**        | **Local**        |
| Shared Level View Bookmarks                                                                          | **Stored**            | **Shared**       |
| Local Level View Bookmarks                                                                           | **Stored**            | **Local**        |

#### Local Bookmarks vs Shared Bookmarks

The only difference between these two options is where the data would be stored. If we want to store a bookmark locally we would store it in the Settings Registry (project/user/Registry/ViewBookmarks), whereas if we want to make it shared we would store it in the prefab itself.

### Bookmark Widget

![Bookmark Widget](https://user-images.githubusercontent.com/82394219/149931093-d0eed8ee-760c-4141-ac96-6fc808d74ef5.png)

### Technical design description

- The first step will be creating a transient View Bookmark. This Bookmark will be stored in memory and we will use it to keep the camera transform every time we go out of Game Mode or Simulate Mode. [#2917](https://github.com/o3de/o3de/issues/2917)

- Then, we'll have to create a system where every time the user closes a level a View Bookmark gets saved to the Editor Settings Registry and every time the user opens a level it loads the transform stored in that View Bookmark. There should also an option to let the user keep the current transform of the camera when switching between levels. 

This is our MVP.

- Using the approach as per the last known camera level location feature, we will implement the capability of storing several View Bookmarks for the level in the Editor Settings Registry. This will become the local View Bookmarks. We will create a 'LocalViewBookmarkComponent' that stores the reference to the settings registry file in the prefab.

- The next step is creating the Shared View Bookmarks. A 'SharedViewBookmarkComponent' will be created that would be part of the Root Entity of the prefab and will hold a list of View Bookmarks saved by the user. Ideally, we would use the same API used by the previous system.
The Bookmarks can be stored via a context menu in the viewport. We will also allow the user to cycle between cameras. We have to make sure we display these options clearly in the UX.

- We will allow the user to create tags for the View Bookmarks so it is easier to organize and access these Bookmarks from the widget. For example, the users could have a "Spawn" tag and, when they filter for that tag, only the Bookmarks that have that will be displayed.

![Bookmark System Diagram](https://user-images.githubusercontent.com/82394219/149931167-22926b3f-1d51-47b9-ab61-a362adc7dfe0.png)


```Cpp
struct ViewBookmark
{
    AZ::Name m_cameraName;
    AZ::Transform m_cameraTransform;
    // This can be extended to contain more data
    // e.g. isOrthographic, cameraFov, etc
}
 
//! Interface for ViewBookmarkSystemComponentNotifications, which is the EBus that dispatches changes to listeners.
class ViewBookmarkSystemComponentNotifications
    : public ComponentBus
{
    //! Called when a new ViewBookmark is added.
    void OnNewViewBookmarkAdded(const ViewBookmark& savedBookmark){};
 
    //! Called when a new ViewBookmark is removed.
    void OnViewBookmarkRemoved(const ViewBookmark& removedBookmark){};
 
    //! Called when a ViewBookmark is modified.
    void OnViewBookmarkUpdated(const ViewBookmark& modifiedBookmark){};
};
 
//! Interface for ViewBookmarkSystemComponentRequestBus, which is an EBus that receives requests
//! to save, remove and modify Bookmarks.
class ViewBookmarkSystemComponentRequestBus
    : public ComponentBus
{
    //! Saves a new ViewBookmark.
    bool AddViewBookmark(ViewBookmark newBookmark) = 0;
 
    //! Removes a ViewBookmark.
    bool RemoveViewBookmark(ViewBookmark bookmarkToRemove) = 0;
 
    //! Modifies an existing ViewBookmark.
    bool UpdateViewBookmark(ViewBookmark BookmarkToModify , AZ::Transform newTransform) = 0;
};
 
class ViewBookmarkSystemComponent
    : public AzToolsFramework::Components::EditorComponentBase
    , public ViewBookmarkSystemComponentRequestBus::Handler
    , public ViewBookmarkSystemComponentNotifications::Handler
{
public:
    ViewBookmarkSystemComponent();
 
    static void Reflect(AZ::ReflectContext* context);
     
    ////////////////////////////////////////////////////////////////////
    // ViewBookmarkSystemComponentRequestBus
 
    //! Adds a new ViewBookmark.
    bool AddViewBookmark(ViewBookmark newBookmark);
 
    //! Removes a ViewBookmark.
    bool RemoveViewBookmark(ViewBookmark bookmarkToRemove);
 
    //! Updates an existing CameraBookmark.
    bool UpdateViewBookmark(ViewBookmark BookmarkToModify, AZ::Transform newTransform);
 
    ////////////////////////////////////////////////////////////////////
    // ViewBookmarkSystemComponentNotifications
     
    //! Called when a new ViewBookmark is added.
    void OnNewViewBookmarkAdded(const ViewBookmark& savedBookmark);
 
    //! Called when a new ViewBookmark is removed.
    void OnViewBookmarkRemoved(const ViewBookmark& removedBookmark);
 
    //! Called when a ViewBookmark is updated.
    void OnViewBookmarkUpdated(const ViewBookmark& modifiedBookmark);
 
private:
    AZStd::vector<ViewBookmark> m_viewBookmarks;
};
```

#### Storing the data

This is an example of how the Bookmark data would look like stored in the prefab

```json
{
    "ContainerEntity": {
        "Id": "Entity_[1146574390643]",
        "Name": "Level",
        "Components":
        {
            "Component_[5126822819577294041]": {
                "$type": "ViewBookmarkComponent",
                "Id": 5126822819577294041,
                "ViewBookmarks": {
                    "LocalBookmarkFileName": "Level1639763579377.setreg",
                    "ViewBookmarks": [
                        {
                            "name": "MobSpawn_1",
                            "tags": ["Spawns"],
                            "Position": [
                                10.12,
                                140.0,
                                -45.5
                            ],
                            "Rotation": [
                                3.13,
                                -17.54,
                                46.98
                            ]
                        },
                        {
                            "name": "MobSpawn_2",
                            "tags": ["Spawns", "Combat"],
                            "Position": [
                                10.0,
                                20.0,
                                30.0
                            ],
                            "Rotation": [
                                30.0,
                                20.0,
                                10.0
                            ]
                        }
                    ]
                }
            },

        }
    },
    "Entities": {...},
    "Instances": {...}
}
```
and in the Settings Registry for the TestViewBookmarkLevel

```json
{
    "TestViewBookmarkLevel1889822833412.setreg": {
        "LocalBookmarks": [
            {
                "name": "MobSpawn_1",
                "tags": ["Spawns"],
                "Position": [
                    24.0,
                    556.97998046875,
                    14.899999618530272
                ],
                "Rotation": [
                    -87.0,
                    -45.97999954223633,
                    4.900000095367432
                ]
            },
            {
                "name": "MobSpawn_2",
                "tags": ["Spawns", "Combat"],
                "Position": [
                    -54.0,
                    12.97998046875,
                    14.899999618530272
                ],
                "Rotation": [
                    -76.0,
                    -45.97999954223633,
                    4.900000095367432
                ]
            }
        ],
        "LastKnownLocation": {
            "Position": [
                199.0,
                1224.8900146484375,
                4413.0
            ],
            "Rotation": [
                10.0,
                20.88999938964844,
                0.0
            ]
        }
    }
}
```

### What are the advantages of the feature?

This feature will improve workflows such as level design and environmental design by making them much more agile, allowing the users to edit, store and organize several camera properties of a level. It will also address several issues that are currently present in the engine.

#### Issues that will be addressed with this feature:

- [ Switch Camera options are not functional #6193 ](https://github.com/o3de/o3de/issues/6193)

- [ Editor Camera is in a different view after exiting maximized game mode view #2917 ](https://github.com/o3de/o3de/issues/2917)

- [ Restore Viewport Camera on Game Mode Exit setting has no effect #2324 ](https://github.com/o3de/o3de/issues/2324)

- [ Camera location/rotation saves when saving level. #2074 ](https://github.com/o3de/o3de/issues/2074)

### What are the disadvantages of the feature?

- If this feature breaks it will affect mostly artists and designers that might lose several or all of the View Bookmarks they have saved which could severely impact in their workflow.
  
- This feature could potentially generate more merge conflicts if different users save different Bookmarks in the prefab which could also lead to duplicate data or having very similar View Bookmarks saved into the same prefab. This issue should be mitigated by having a Bookmark picking widget that allows the user to select which Bookmarks should be local and which should be shared.

### How will this be implemented or integrated into the O3DE environment?

The main part would be implementing this `ViewBookmarkSystemComponent`.

### Are there any alternatives to this feature?

As mentioned before, we currently have the "Viewport Camera Selector" but this is a feature wasn't meant to be utilized in this context and users are just using it due to not having a proper feature for these kind of workflows.

### How will users learn this feature?

- We can add some docs as well as demonstrate its utility through tutorial videos.
- Move the Bookmark selection widget near the existing camera settings.

### Are there any open questions?
- Will this feature replace the current Viewport Selector and _Be This Camera_ features?
   - No, it will not replace any existing feature and we are not going to remove anything. The main purpose of this feature is replacing a lost functionality from the old Cry Viewport that let the user save camera positions (function keys)
- How are we going to support multiple viewports?
  - This should be easily achieved as the viewport that is in focus would be the one that will be loading the Location Bookmark data.
- How are we storing the data?
  - As far as data storage relates, we want to follow o3de standards (project/user folder). We like the idea of having custom stereg files instead of storing this data in already existing ones. This will provide a better encapsulation keeping the files clean and accessible.

## Changelog
[2020/02/03] - Renamed Camera Bookmark RFC to View Bookmark RFC to clarify feature scope.

[2020/03/07] - Updated with new data from investigating the issue.
