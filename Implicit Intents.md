# Implicit Intents

In this section you'll learn more about implicit intents, and how you can use them to activate activities as well.

Implicit intents allow you to activate an activity if you know the action, but not the specific app or activity that will handle that action. For example, if you want your app to take a photo, or send email, or display a location on a map, you typically do not care which specific app or activity actually performs these actions.

Conversely, your activities can declare one or more intent filters in the Android manifest that advertise that activity's ability to accept implicit intents and to define the particular type of intents it will accept.

To match your request with a specific app installed on the device, the Android system matches your implicit intent with an activity whose intent filters indicate that they can perform that action. If there are multiple apps installed that match, the user is presented with an app chooser that lets them select which app they want to use to handle that intent.

In this practical you'll build an app that sends three implicit intents: to open a URL in a web browser, to open a location on a map, and to share a bit of text. Sharing -- sending a piece of information to other people through email or social media -- is a common and popular feature in many apps. For the sharing action we'll use the ShareCompat.IntentBuilder class, which makes it easy to build intents for sharing data.

Finally, we'll create a simple intent receiver app that accepts implicit intents for a specific action.

### App overview

In this section you'll create a new app with one activity and three options for actions: open a web site, open a location on a map, and share a snippet of text. All of the text fields are editable (EditText), but contain default values.

![](https://google-developer-training.github.io/android-developer-fundamentals-course-practicals/images/2_3_P_images/pv_implicitintents-screenshot.png)

