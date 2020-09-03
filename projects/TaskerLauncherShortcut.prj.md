# TaskerLauncherShortcut

## Export Info:
**Tasker Version:** `5.9.3.beta.5`  
**Timestamp:** `2020-05-16 21.58.26`  
**sha256sum:** `7c8a6f69c201538834f5df941f237222854c72aade8ee01de0578f62804b3045`  



## Profile Names:
**Count:** `0`




## Scene Names:
**Count:** `0`




## Task Names:
**Count:** `5`

- *`Get Intent Info From Intent Uri`*
- *`Get And Set Default Launcher`*
- *`TaskerLauncherShortcut Non DEEP_SHORTCUT Template`*
- *`Send Shortcut Intent With TaskerLauncherShortcut`*
- *`TaskerLauncherShortcut DEEP_SHORTCUT Template`*



## Profiles Info:



## Tasks Info:

**#:** `1`  
**Name:** `Get Intent Info From Intent Uri`  
**ID:** `901`  
**Collision Handling:** `Run Both Together`  
**Keep Device Awake:** `false`  
**Help:**
````
A task that converts an intent Uri back to an intent and gets all its info. The task also dynamically generates an "am start" command for the intent so that it may be run with a shell, like with the "Run Shell" action.

Intents for shortcuts are normally stored as Uri strings by launcher apps by running the "Intent.toUri()" function on the intent passed by the apps to them when the shortcut is first created. This Uri can be converted back to an intent by running the "Intent.getIntent(String)" function on the Uri string. For example in nova launcher, the shortcut intent Uris are stored in "/data/data/com.teslacoilsw.launcher/databases/launcher.db" -> "favorites" table. It would require root to access such data.

The implementation of "Intent.toUri()" is found at the following link:
https://github.com/aosp-mirror/platform_frameworks_base/blob/nougat-release/core/java/android/content/Intent.java#L8433

The implementation of "Intent.getIntent(String)" is found at the following link:
https://github.com/aosp-mirror/platform_frameworks_base/blob/nougat-release/core/java/android/content/Intent.java#L4982

The intent can normally be run by the "Shortcut" action by passing it the intent Uri.

The intent can also be run by running "CONTEXT.startActivity(Intent)" on the intent object returned by the "Intent.getIntent(String)" function. You may optionally override the flags and other data of the intent depending on your needs. There are 3 disabled actions after the "Convert Intent Uri To Intent Object" anchor that shows how to do it.

However, in some cases a user would like to understand how exactly an intent works so he may modify it for other use cases or maybe he wants to run the same intent with an am command using a shell instead of using java. This task was mainly created for that purpose. This task extracts info from an intent object created from an intent uri which may include its action, type, scheme, package, component, flags, category and extras. The extra keys in the intent uri begin with a prefix which define the type of the extra. "S." -> String, "B." -> Boolean, "b." -> Byte, "c." -> Char, "d." -> Double, "f." -> Float, "i." -> Integer, "l." -> Long, "s." -> Short. Other types are not supported, like Uri and lists. Intent sourceBounds and selector are not extracted by this task since there are not needed for background commands. Using this info an am command is dynamically generated for the user so that the it can be run using a shell, like with the "Run Shell" action.

With the intent info, the user can technically do at least 2 things. Either use the "Send Intent" action or use the am command to send the intent. Both have their drawbacks and benefits.

The "Send Intent" action only allows a maximum of 3 extras, but according to tasker docs, those extras can be type casted to more types than the am command supports, mainly the type double, char, byte and short which the am command does not support. However, flags or multiple categories can't be sent with the "Send Intent" action.

The am command supports extras of type string, bool, int, long and float. It also support multiple extras and categories.

So depending on the use case of the user, he can use either since neither is "one to rule them all". Using java actions would be.

For more info on intents, check the following links:

https://developer.android.com/reference/android/content/Intent

https://tasker.joaoapps.com/userguide/en/intents.html


To use this task, set the required variables in the "Set User Modifiable Variables*" section of this task.

The intent_uri variable should be set to the intent Uri whose info needs to be extracted, like a shortcut intent. It can optionally be passed as %par1 which will override the variable set action. Default value is reddit app shortcut.

The copy_intent_info_to_clipboard is a toggle that decides if the intent info should be copied to the clipboard. Default value is "1".

The run_am_start_command is a toggle that decides whether the "am start" command that is dynamically generated should be run at the end of this task. Default value is "1".

The override_am_command_flags_value is a toggle that decides whether the flag stored in the intent should be overridden by the value "335544320" when the "am start" command is generated. The flag value "335544320 (0x14000000)" is basically an OR of "FLAG_ACTIVITY_NEW_TASK 268435456 (0x10000000)" and "FLAG_ACTIVITY_CLEAR_TOP 67108864 (0x04000000)". It is needed to create another activity stack when the new activity is started, like when you want to start a new app. The tasker `Shortcut` action also sends the same flags and ignores the one in the intent uri, at least in my minimal testing. Default value is "1".

The am command generated may not work for all cases. If an extra is null or of a type not supported by the am command, then a warning is flashed and intent will not be sent. Any parameters that need to be passed to the am command that contain a single quote are automatically escaped.


Input %par1: #optional
"
intent_uri
"

Returns:
"
intent_info
"

If task is successful, then intent_info will contain the intent info and am command.
Otherwise it will not be set.
````
##


**#:** `2`  
**Name:** `Get And Set Default Launcher`  
**ID:** `874`  
**Collision Handling:** `Run Both Together`  
**Keep Device Awake:** `false`  
**Help:**
```
A helper task that gets or sets the default android launcher app with commands. It requires either root or adb. If you don't have either of those, then it is not possible to set the default launcher with background commands, other than tasker being set the device owner (not device admin) but that way has some issues that need to be sorted out. The mode is automatically chosen depending on whatever is available in the order "root, adb". If none of them is detected, then the task exits with failure.

The action passed to this task as %par1 must be set to either "get" or "set". If no parameter is passed, then "get" is automatically assumed.

The "get" action can be used to get the normal_default_launcher_package_and_activity_name for the "Send Shortcut Intent With TaskerLauncherShortcut" task. If this task is run directly without any parameters, then it automatically copies the current default launcher to the clipboard.


Input %par1: #optional
"
action
"

Returns:
"
launcher_package_and_activity_name/result_code #depending on action
"

If action is "get" and task is successful, then launcher_package_and_activity_name will contain the default launcher package and activity name.
Otherwise result will not be set.

If action is "set" and task is successful, then result_code will contain the "0".
Otherwise it will contain "1".

For other cases, result will not be set.
```
##


**#:** `3`  
**Name:** `TaskerLauncherShortcut Non DEEP_SHORTCUT Template`  
**ID:** `958`  
**Collision Handling:** `Abort New Task`  
**Keep Device Awake:** `false`  
**Help:**
```
A template task to send a Non DEEP_SHORTCUT shortcut intent Uri with TaskerLauncherShortcut plugin. The default value will only work for android less than 7.1 (API 25).


This task does not take any parameters or return anything.
```
##


**#:** `4`  
**Name:** `Send Shortcut Intent With TaskerLauncherShortcut`  
**ID:** `925`  
**Collision Handling:** `Run Both Together`  
**Keep Device Awake:** `false`  
**Help:**
```
A task that starts a launcher shortcut using the TaskerLauncherShortcut plugin.

In Android 7.1(API 25) new [ShortcutManager](https://developer.android.com/reference/android/content/pm/ShortcutManager) APIs were added for apps to create shortcuts and [LauncherApps](https://developer.android.com/reference/android/content/pm/LauncherApps) APIs for launcher apps to access shortcuts.

There are mainly 3 types of shortcuts, static, dynamic and pinned. Static remain the same and are mainly declared in the android manifest of the app. Dynamic shortcuts are published by apps at runtime to ShortcutManager and displayed by launchers like nova launcher by long pressing the app icon. Pinned icons are shortcuts sent by apps to the launcher when you press buttons like "Add to Home screen", like pinning a website or chat shortcut on the launcher home. The static and pinned shortcuts also existed before android 7.1 but used different ways to publish or create shortcuts and they could be started by any app by sending intents through java or am commands. However, the dynamic and pinned shortcuts and some static shortcuts for android 7.1 and higher can only be started by the default launcher apps or currently active voice interaction service. The permission can be checked by an app using [hasShortcutHostPermission](https://developer.android.com/reference/android/content/pm/LauncherApps#hasShortcutHostPermission()). If the app doesn't have the permission, the shortcut intent's desired action will not be successful, even though the target app may open. These shortcuts contain a special category called "com.android.launcher3.DEEP_SHORTCUT" and also have an string extra called "shortcut_id" which defines the id with which the shortcut is registered under the ShortcutManager. Moreover, dynamic shortcuts published by apps can't even be queried by apps and nor can pinned shortcuts be received by apps that don't have the required permission. Another point is that these shortcuts can't be used in android less than 7.1 since ShortcutManager doesn't exist, since the real intent and it's extras are stored by the ShortcutManager when dynamic shortcuts are published by the apps and launcher apps don't have access to them, they only receive a shortcut_id from which the shortcut could be started using the LauncherApps startShortcut() API. Another thing is that there is a way for static shortcuts that only show in android 7.1 and higher to be shown in older devices as well, since nova launcher does it, but this has to yet to be investigated and the TaskerLauncherShortcut plugin doesn't support it currently.

For these reasons the Tasker "Shortcut" action, AutoShortcut, java intents or am command is not going to work in android 7.1 and higher for DEEP_SHORTCUTS. What can be done is either ask your default launcher dev to add support for a tasker plugin or intent that may be used to start intents stored in the launcher or use the TaskerLauncherShortcut app plugin which takes a shortcut intent Uri as input and starts the shortcut. The TaskerLauncherShortcut is a launcher app and created mainly for starting shortcuts and has a plugin that can be used with tasker. It should ideally support all android versions greater than or equal to 4.1 (API 16). It's a very basic launcher and the homescreen only shows a list of installed apps that can be started on click and does not support adding shortcuts to homescreen. It's options menu supports changing the default launcher by showing the android's "Choose Default Home" screen and also supports searching for static, dynamic and pinned shortcuts depending on android version, the selected shortcut's intent Uri is only copied to the clipboard for other uses like using it in the plugin inside Tasker. The pinned shortcut are of course only received by the launcher app when an app sends them and can't be searched. Previously pinned shortcuts can technically be shown but are not.

Now as already mentioned that DEEP_SHORTCUTS requires the app starting the shortcut to be the default launcher app, so the TaskerLauncherShortcut must be the default launcher app when the plugin is called. To change the default launcher app without using touch simulation or Autoinput and just using background commands requires either root or adb. The "cmd package set-home-activity %launcher_package_and_activity_name" command can be used for this. Setting tasker as the device owner (not device administrator) may work but requires more work. The "Send Shortcut Intent With TaskerLauncherShortcut" task is provided by the "TaskerLauncherShortcut" tasker project that takes cares of everything for the user. It takes an intent Uri as input %par1 and if it's a DEEP_SHORTCUT, then it automatically sets the "TaskerLauncherShortcut" app as the default launcher, starts the shortcut and then resets the user's normal launcher app as the default launcher. The default launchers are set by the "Get And Set Default Launcher" task. This is done in the background and does not require any interaction. Changing the default launcher for shortcuts that are not DEEP_SHORTCUT is not required and so neither is root or adb and you may use the plugin directly in any task as a standalone action instead of using the "Send Shortcut Intent With TaskerLauncherShortcut" task. The plugin also has an internal "Search Shortcuts" button when you open the plugin configuration while editing the plugin action that automatically sets the plugin input field with the intent Uri. The plugin action in the "Send Shortcut Intent With TaskerLauncherShortcut" task uses a local variable so that same action can be used dynamically for different intents received as parameters to the task, do not edit it. A template task "TaskerLauncherShortcut Non DEEP_SHORTCUT Template" is given for this.

To use DEEP_SHORTCUT shortcuts, you must do two things:

1. You need to set the "%normal_default_launcher_package_and_activity_name variable" in the "Send Shortcut Intent With TaskerLauncherShortcut" task. It defines the package and activity name of the normal default launcher that should be reverted back to after the shortcut has been sent. By default this is set to nova launcher. If you use a different launcher, then just run the "Get And Set Default Launcher" task directly from the tasker UI and the package and activity name of your current launcher will be copied to the clipboard. Paste that in the "Variable Set" action of the "%normal_default_launcher_package_and_activity_name" variable in the "Send Shortcut Intent With TaskerLauncherShortcut" task.

2. You need a find the shortcut intent Uri that needs to be passed to the "Send Shortcut Intent With TaskerLauncherShortcut" task as %par1. Open the "TaskerLauncherShortcut" app, and from its options, click "Search Shortcuts". You will be asked to set the app as the default launcher. Make sure to select "Always" or "Use as default app" instead of "Just Once", depending on android version. Press Home button to make sure its set as the default and that no prompt is shown. Once its set, then return to the Shortcut Chooser activity. For static and dynamic shortcuts, just clicking on the shortcut will copy the Uri. For static shortcuts, you may be taken to a configuration screen. For pinned shortcuts, you will be asked to go to the app for which you want to create a pinned shortcut for and create it and then return. Once you go to the app like chrome and click something like "Add to Home screen" and return to Shortcut Chooser activity from recents menu, the shortcut intent Uri will be copied to the clipboard. Use that intent Uri in a tasker task and send it as %par1 to the "Send Shortcut Intent With TaskerLauncherShortcut" task with a "Perform Task" action to start the shortcut. A template task "TaskerLauncherShortcut DEEP_SHORTCUT Template" is given for this.


The shortcut_intent_uri passed to this task as %par1 must be set to a valid intent Uri.

The %normal_default_launcher_package_and_activity_name must be set in the format "package_name/activity_name"


Input %par1: #optional
"
shortcut_intent_uri
"

Returns:
"
result_code
taskerlaunchershortcut_errmsg #optional
"

If task is successful, then result_code will contain 0.
Otherwise it will contain appropriate exit code.

taskerlaunchershortcut_errmsg will contain %errmsg if the plugin action failed.
```
##


**#:** `5`  
**Name:** `TaskerLauncherShortcut DEEP_SHORTCUT Template`  
**ID:** `955`  
**Collision Handling:** `Abort New Task`  
**Keep Device Awake:** `false`  
**Help:**
```
A template task that runs the "Send Shortcut Intent With TaskerLauncherShortcut" task to send a DEEP_SHORTCUT shortcut intent Uri with TaskerLauncherShortcut plugin. The default value will only work for android greater than 7.1 (API 25). Kiwi Browser must also be installed for default value to work.


This task does not take any parameters or return anything.
```
##

