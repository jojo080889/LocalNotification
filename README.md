LocalNotification Plugin for PhoneGap
==========================
This PhoneGap plugin for Android is based on [the phonegap plugin found here](https://github.com/phonegap/phonegap-plugins/tree/master/Android/LocalNotification), and the tips found [at this blog post](http://tech.cibul.net/using-the-android-localnotification-plugin-for-phonegap/), updated to work with PhoneGap 3. This plugin must be manually installed (for now).

I had to update the old plugin for a project of mine, so I figured I would share the code and steps I took so others could use the plugin too :)

## Manual Installation for Android

1) Create a basic phonegap project using the PhoneGap CLI.

2) Add **LocalNotification.js** to your **<project directory>/www/js** directory.

3) In **<project directory>/platforms/android/src/**, create a **com** folder if it doesn't exist. Inside of that, create a **phonegap** folder. Inside of that, create a **plugin** folder. Finally, inside of that, create the **localnotification** folder.

4) Copy all the .java files into this last folder.

5) Open **AlarmReceiver.java** and change **import com.phonegap.your.project.namespace.R;** so that it uses your app's package name instead. Remember to keep the .R at the end of the line!

6) Open **<project directory>/platforms/android/res/xml/config.xml** and add the following to the end of the <feature> list (inside the <widget> tag):

```xml
<feature name="LocalNotification">
  <param name="android-package" value="com.phonegap.plugin.localnotification.LocalNotification" />
  </feature>
```

7) Open **<project directory>/platforms/android/AndroidManifest.xml** and add the following inside the <application> tag:

```xml
<receiver android:name="com.phonegap.plugin.localnotification.AlarmReceiver" >
</receiver>

<receiver android:name="com.phonegap.plugin.localnotification.AlarmRestoreOnBoot" >
  <intent-filter>
    <action android:name="android.intent.action.BOOT_COMPLETED" />
  </intent-filter>
</receiver>
```

8) Lastly, reference **LocalNotification.js** in your **www/index.html** or wherever your main application code is:

```html
<script type="text/javascript" src="js/LocalNotification.js"></script>
```

## Usage
Here's an example of a notification being set to go off a minute from now:

```js
var d = new Date();
d = d.getTime() + 3*1000;
d = new Date(d)

plugins.localNotification.add({
  date : d,             
  message : "Phonegap - Local Notification\r\nSubtitle comes after linebreak",
  ticker : "This is a sample ticker text",
  repeatDaily : false,
  id : 4
});
```

Some other functions you can use:
```js
plugins.localNotification.add({ date: new Date(), message: 'This is an Android alarm using the statusbar', id: 123 });

plugins.localNotification.cancel(123); 

plugins.localNotification.cancelAll();
```

## Other Tips
* This plugin must be added manually and doesn't work with PhoneGap Build. This means you have to use the CLI to build and deploy your app (use **phonegap local run android**)
