# Permissions Management in Android 6

If you work with mobile or hybrid applications, you might have already heard about some changes regarding permissions management in Android 6. But how they affect you as a developer for hybrid applications? Do you need to worry about them? In my case those changes broke the app, so read on if you don't want to be surprised at some point with yours.

### What has changed?

With the introduction of Android 6, some major changes in permission management have been introduced. First of all, prior to that version, Android itself was responsible for asking the users for permissions - all you needed to do was define what you need. The consequence was that either the user accepts all your app wants, or they have to cancel the installation. This was sometimes a bit frustrating for the user, I must admit, as you might be OK with allowing access to eg. setting an alarm, but not quite with sharing contacts' details. As for the developer, you never had to check for anything, as it was safe to assume, that once the user enters an app, they have already agreed to everything.

The latest Android enables you (as a user) to pick and choose what permissions will be accessible for an app and which won't. This is very cool, but at the same time it causes some problems for the developers. Why is that? Because from now on, an application has to ask their users for specific permissions **at runtime**, and it might ask them numerous times (eg. every time they reach a point of accessing the camera), until they finally agree (or they select _Never ask again_ option). What is
 very important here is, that your app has to know how to handle rejection! You don't want your app to just crash out of the blue, right?

### Example

I created a simple example application to show how those differences look in practice. An application called _Cordova Permissions Example_ needs to access device's contacts to display its total count. I have created it on top of basic Cordova project, just making a few changes in `index.js` file:

    // ./www/js/index.js
    ...
    navigator.contacts.find(
        [navigator.contacts.fieldType.displayName],
        function(contacts){
            alert('We found ' + contacts.length + ' contacts on your device!');
        }, function(contactError) {
            alert('onError! ' + contactError);
        }, {
            multiple: true
        });
    ...

We also need to add [contacts plugin](https://www.npmjs.com/package/cordova-plugin-contacts) with:

    ordova plugin add cordova-plugin-contacts

Now, when I install it on my Samsung Galaxy S3 (Android 4.3), I have to agree to all permissions beforehand (screen on the left, Cordova requires an Internet connection, so it's on the list, too). I can also verify what I nodded to via application settings (right):

<img src="https://github.com/mycodesmells/cordova-permissions-example/blob/master/posts/images/android-43-install.png"/>

Then at runtime we don't need to worry about those permissions at all, as we get a popup with contacts count immediately:

<img src="https://github.com/mycodesmells/cordova-permissions-example/blob/master/posts/images/android-43-result.png"/>

Now, on Nexus 5X (with Android 6.0) it's slightly more complicated. During an installation, we don't accept anything (screen on the left), but then at the same time no permissions are enabled, as you can see via application settings (centre and right):

<img src="https://github.com/mycodesmells/cordova-permissions-example/blob/master/posts/images/android-60-install.png"/>

But what happens when we jump into the app? The moment we reach the point to access deviece's contacts, our plugin asks us to agree if it can access them. This is new (and you need to get used to see those prompts every now and then if you switch to Android 6), but once you accept this request, we get a number of contacts:

<img src="https://github.com/mycodesmells/cordova-permissions-example/blob/master/posts/images/android-60-runtime.png"/>

### Consequences

As you can see, it worked both on Android 4.3 and 6.0, without any changes to JS code. This is smooth, thanks to Cordova. It serves here as a middleware between your code and the device, but this means a lot for plugin developers. If you take a look [at Cordova docs](https://cordova.apache.org/docs/en/latest/guide/platforms/android/plugin.html#runtime-permissions-cordova-android-500), it has been mentioned there that making your plugin work with Android 6 will require a bit more effort. To check if your plugin works with Android 6, you should search for `requestPermission` function somewhere in its source code, and make sure that it follows Cordova docs on this topic.

### Summary

In my case, I have been using a custom-designed plugin that was lot prepared to see those changes in the latest Android. Because of that, the app never asked me for any permissions, yet it kept crashing time and time again. If you don't want the same to happen to you, do check your plugins!

Source code for this simple example is available [on Github](https://github.com/mycodesmells/cordova-permissions-example).
