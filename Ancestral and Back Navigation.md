# Ancestral and Back Navigation

### Providing users with a path through your app
In the early stages of developing an app, you should determine the path you want users to take through your app to do each task. (The tasks are things like placing an order or browsing content.) Each path enables users to navigate across, into, and out of the tasks and pieces of content within the app.

Often you need several paths through your app that offer the following types of navigation:

- Back navigation, where users navigate to the previous screen using the Back button.
- Hierarchical navigation, where users navigate through a hierarchy of screens. The hierarchy is organized with a parent screen for every set of child screens.

### Back-button navigation

Back-button navigation—navigation back through the history of screens—is deeply rooted in the Android system. Android users expect the Back button in the bottom left corner of every screen to take them to the previous screen. The set of historical screens always starts with the user's Launcher (the device's Home screen), as shown in the figure below. Pressing Back enough times should return the user back to the Launcher.

![](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/images/4-4-c-user-navigation/dg_temporal_navigation.png)

In the figure above:

- Starting from Launcher.
- Clicking the Back button to navigate to the previous screen.
You don't have to manage the Back button in your app. The system handles tasks and the back stack—the list of previous screens—automatically. The Back button by default simply traverses this list of screens, removing the current screen from the list as the user presses it.

There are, however, cases where you may want to override the behavior for the Back button. For example, if your screen contains an embedded web browser in which users can interact with page elements to navigate between web pages, you may wish to trigger the embedded browser's default back behavior when users press the device's Back button.

The onBackPressed() method of the Activity class is called whenever the Activity detects the user's press of the Back key. The default implementation simply finishes the current Activity, but you can override this to do something else:

```java
@Override
public void onBackPressed() {
    // Add the Back key handler here.
    return;
}
```

If your code triggers an embedded browser with its own behavior for the Back key, you should return the Back key behavior to the system's default behavior if the user uses the Back key to go beyond the beginning of the browser's internal history.

### Hierarchical navigation patterns

To give the user a path through the full range of an app's screens, the best practice is to use some form of hierarchical navigation. An app's screens are typically organized in a parent-child hierarchy, as shown in the figure below:

![](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/images/4-4-c-user-navigation/dg_app_hierarchy.png)

In the figure above:

- Parent screen
- First-level child screen siblings
- Second-level child screen siblings

#### Parent screen
A parent screen (such as a news app's home screen) enables navigation down to child screens.

- The main Activity of an app is usually the parent screen.
- Implement a parent screen as an activity with descendant navigation to one or more child screens.

#### First-level child screen siblings
Siblings are screens in the same position in the hierarchy that share the same parent screen (like brothers and sisters).

- In the first level of siblings, the child screens may be collection screens that collect the headlines of stories, as shown above.
- Implement each child screen as an Activity or Fragment.
- Implement lateral navigation to navigate from one sibling to another on the same level.
- If there is a second level of screens, the first level child screen is the parent to the second level child screen siblings. Implement descendant navigation to the second-level child screens.

#### Second-level child screen siblings
In news apps and others that offer multiple levels of information, the second level of child screen siblings might offer content, such as stories.

- Implement each second-level child screen sibling as another Activity or Fragment.
- Stories at this level may include embedded story elements such as videos, maps, and comments, which might be implemented as fragments.
You can enable the user to navigate up to and down from a parent, and sideways among siblings:

- Descendant navigation: Navigating down from a parent screen to a child screen.
- Ancestral navigation: Navigating up from a child screen to a parent screen.
- Lateral navigation: Navigating from one sibling to another sibling (at the same level).
You can use the main Activity of the app as a parent screen, and then add an Activity or Fragment for each child screen.

### Main Activity with an activity for each child
If the first-level child screen siblings have another level of child screens under them, you should implement each first-level screen as an activity, so that the lifecycle of each screen is managed properly before calling any second-level child screens.

For example, in the figure above, the parent screen is most likely the main activity. An app's main activity (usually MainActivity.java) is typically the parent screen for all other screens in your app. You implement a navigation pattern in the main activity to enable the user to go to another activity or fragment. For example, you can implement navigation using an Intent that starts an Activity.

**Tip**: Using an Intent in the current activity to start another activity adds the current activity to the call stack, so that the Back button in the other activity (described in the previous section) returns the user to the current activity.

As you've learned, the Android system initiates code in an Activity with callback methods that manage the Actactivityivity lifecycle for you. (A previous lesson covers the activity lifecycle; for more information, see Activities in the Android developer documentation.)

The declaration of each child activity is defined in the AndroidManifest.xml file with its parent activity. For example, the following defines OrderActivity as a child of the parent MainActivity:

```java
<activity android:name=".OrderActivity"
   android:label="@string/title_activity_order"
   android:parentActivityName=
                        "com.example.android.droidcafe.MainActivity">
   <meta-data
      android:name="android.support.PARENT_ACTIVITY"
      android:value=".MainActivity"/>
</activity>
```

### Main Activity with a Fragment for each child
If the child screen siblings do not have another level of child screens under them, you can define each one as a Fragment, which represents a behavior or portion of a UI within in an activity. Think of a fragment as a modular section of an activity which has its own lifecycle, receives its own input events, and which you can add or remove while the activity is running.

You can add more than one fragment in a single activity. For example, in a section sibling screen showing a news story and implemented as an Activity, you might have a child screen for a video clip implemented as a Fragment. You would implement a way for the user to navigate to the video clip Fragment, and then back to the Activity that shows the story.

### Ancestral navigation (the Up button)
With ancestral navigation in a multitier hierarchy, you enable the user to go up from a section sibling to the collection sibling, and then up to the parent screen.

![](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/images/4-4-c-user-navigation/dg_app_hierarchy_ancestral_navigation.png)

In the figure above:

- Up button for ancestral navigation from the first-level siblings to the parent.
- Up button for ancestral navigation from second-level siblings to the first-level child screen acting as a parent screen.
The Up button is used to navigate within an app based on the hierarchical relationships between screens. For example (referring to the figure above):

- If a first-level child screen offers headlines to navigate to second-level child screens, the second-level child screen siblings should offer Up buttons that return to the first-level child screen, which is their shared parent.
- If the parent screen offers navigation to first-level child siblings, then the first-level child siblings should offer an Up button that returns to the parent screen.
- If the parent screen is the topmost screen in an app (that is, the app's home screen), it should not offer an Up button.
**Tip**: The Back button below the screen differs from the Up button. The Back button provides navigation to whatever screen you viewed previously. If you have several children screens that the user can navigate through using a lateral navigation pattern (as described later in this chapter), the Back button would send the user back to the previous child screen, not to the parent screen. Use an Up button if you want to provide ancestral navigation from a child screen back to the parent screen. For more information about Up navigation, see Providing Up Navigation. See the concept chapter on menus and pickers for details on how to implement the app bar.

To provide the Up button for a child screen Activity, declare the parent of the Activity to be the main Activity in the AndroidManifest.xml file:

```java
<activity android:name="com.example.android.droidcafeinput.OrderActivity"
    android:label="Order Activity"
    android:parentActivityName=".MainActivity">
    <meta-data android:name="android.support.PARENT_ACTIVITY"
        android:value=".MainActivity"/>
</activity>
```

The snippet above in AndroidManifest.xml declares the parent for the child screen OrderActivity to be MainActivity. It also sets the android:label to a title for the Activity screen to be "Order Activity". The child screen now includes the Up button in the app bar (highlighted in the figure below), which the user can tap to navigate back to the parent screen.

![](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/images/4-4-c-user-navigation/up_navigation_in_app_bar.png)

### Descendant navigation
With descendant navigation, you enable the user to go from the parent screen to a first-level child screen, and from a first-level child screen down to a second-level child screen.

![](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/images/4-4-c-user-navigation/dg_app_hierarchy_descendent_navigation.png)

In the figure above:

- Descendant navigation from parent to first-level child screen
- Descendant navigation from headline in a first-level child screen to a second-level child screen

### Buttons or targets
The best practice for descendant navigation from the parent screen to collection siblings is to use buttons or simple targets such as an arrangement of images or iconic buttons (also known as a dashboard). When the user touches a button, the collection sibling screen opens, replacing the current context (screen) entirely.

Tip: Buttons and simple targets are rarely used for navigating to section siblings within a collection. See lists, carousels, and cards in the next section.

![](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/images/4-4-c-user-navigation/dg_app-navigation-descendant-buttons.png)

In the figure above:

- Buttons on a parent screen
- Targets (Image buttons or icons) on a parent screen
- Descendant navigation pattern from parent screen to first-level child siblings
A dashboard usually has either two or three rows and columns, with large touch targets to make it easy to use. Dashboards are best when each collection sibling is equally important. You can use a LinearLayout, RelativeLayout, or GridLayout. See Layouts for an overview of how layouts work.





































