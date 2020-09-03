# TaskerLauncherShortcut Tasker Project

`TaskerLauncherShortcut Tasker Project` is a sample project for the [Tasker App] to run [TaskerLauncherShortcut App] shortcut intents. It requires [Tasker App] to be granted either `root` or `ADB` access for launching shortcuts with the `com.android.launcher3.DEEP_SHORTCUT` category in android `>=7.1`.
##


### Contents
- [How Project Works](#How-Project-Works)
- [Compatibility](#Compatibility)
- [Dependencies](#Dependencies)
- [Downloads](#Downloads)
- [Install Instructions For Tasker In Android](#Install-Instructions-For-Tasker-In-Android)
- [Usage](#Usage)
- [Current Features](#Current-Features)
- [Planned Features](#Planned-Features)
- [Issues](#Issues)
- [Worthy Of Note](#Worthy-Of-Note)
- [FAQs And FUQs](#FAQs-And-FUQs)
- [Changelog](#Changelog)
- [Contributions](#Contributions)
##


### How Project Works

As already mentioned in the [TaskerLauncherShortcut App] info, the shortcuts with the `com.android.launcher3.DEEP_SHORTCUT` category require the app starting the shortcut to be the default launcher app or currently be the active voice interaction service, so the `TaskerLauncherShortcut` must be the default launcher app when the plugin is called. To change the default launcher app without using touch simulation or [AutoInput](https://play.google.com/store/apps/details?id=com.joaomgcd.autoinput&hl=en) and just using background commands requires either `root` or `ADB`. The `cmd package set-home-activity %launcher_package_and_activity_name` command can be used for this. Setting tasker as the device owner (not device administrator) may work but requires more work. The `Send Shortcut Intent With TaskerLauncherShortcut` task is provided that takes cares of everything for the user. It takes an intent `Uri` as input `%par1` and if it's a `DEEP_SHORTCUT`, then it automatically sets the `TaskerLauncherShortcut` app as the default launcher, starts the shortcut and then resets the user's normal launcher app as the default launcher. The default launchers are set by the `Get And Set Default Launcher` task. This is done in the background and does not require any interaction. The plugin action in the `Send Shortcut Intent With TaskerLauncherShortcut` task uses a local variable so that same action can be used dynamically for different intents received as parameters to the task, **do not** edit it.

For shortcuts without the `com.android.launcher3.DEEP_SHORTCUT` category, changing the default launcher is not required and so neither is `root` or `ADB` and you may use the plugin directly in any task as a standalone action instead of using the `Send Shortcut Intent With TaskerLauncherShortcut` task. A template task `TaskerLauncherShortcut Non DEEP_SHORTCUT Template` is given for this.

The project and the app has been tested on an Android 7.0 device and Android 8.1 and 10 emulators, but the Android 10 emulator didn't have root and `ADB Wi-Fi` likely can't work, so haven't tested changing the default launcher commands on it, but the intents work. There could still be issues because of android 10 background activity start restrictions but hopefully the `Draw Over Other Apps` permission should fix that.

Check [TaskerLauncherShortcut Project Info](projects/TaskerLauncherShortcut.prj.md) file for more info of the profiles and tasks.
##


### Compatibility

- Android using [Tasker App].
##


### Dependencies

- [TaskerLauncherShortcut App]
- Requires [Tasker App] to be granted either `root` or `ADB` access for launching shortcuts with the `com.android.launcher3.DEEP_SHORTCUT` category in android `>=7.1`.
##


### Downloads

- [GitHub releases](https://github.com/agnostic-apollo/TaskerLauncherShortcut-Tasker-Project/releases).
- [Taskernet](https://taskernet.com/shares/?user=AS35m8mXdvaT1Vj8TwkSaCaoMUv220IIGtHe3pG4MymrCUhpgzrat6njEOnDVVulhAIHLi6BPUt1&id=Project%3ATaskerLauncherShortcut)
##


### Install Instructions For Tasker In Android

1. Import `projects/TaskerLauncherShortcut.prj.xml` Project file into Tasker.

##


### Usage

**DEEP_SHORTCUT shortcuts**:

1. You need to set the `%normal_default_launcher_package_and_activity_name variable` in the `Send Shortcut Intent With TaskerLauncherShortcut` task. It defines the package and activity name of the normal default launcher that should be reverted back to after the shortcut has been sent. By default this is set to nova launcher. If you use a different launcher, then just run the `Get And Set Default Launcher` task directly from the tasker UI and the package and activity name of your current launcher will be copied to the clipboard. Paste that in the `Variable Set` action of the `%normal_default_launcher_package_and_activity_name` variable in the `Send Shortcut Intent With TaskerLauncherShortcut` task.

2. You need to find the shortcut intent `Uri` that needs to be passed to the `Send Shortcut Intent With TaskerLauncherShortcut` task as `%par1`. Open the `TaskerLauncherShortcut` app, and from its options, click `Search Shortcuts`. You will be asked to set the app as the default launcher. Make sure to select `Always` or `Use as default app` instead of `Just Once`, depending on android version. Press `Home` button to make sure its set as the default and that no prompt is shown. Some devices make require going into android settings to set the default launcher. Once its set, then return to the `Shortcut Chooser` activity. For static and dynamic shortcuts, just clicking on the shortcut will copy the intent `Uri`. For static shortcuts, you may be taken to a configuration screen. For pinned shortcuts, you will be asked to go to the app for which you want to create a pinned shortcut for and create it and then return. Once you go to the app like chrome and click something like `Add to Home screen` and return to Shortcut Chooser activity from recents menu, the shortcut intent `Uri` will be copied to the clipboard. Use that intent `Uri` in a tasker task and send it as `%par1` to the `Send Shortcut Intent With TaskerLauncherShortcut` task with a `Perform Task` action to start the shortcut. A template task `TaskerLauncherShortcut DEEP_SHORTCUT Template` is given for this.

**Non DEEP_SHORTCUT shortcuts**:

1. Open the `TaskerLauncherShortcut Non DEEP_SHORTCUT Template` task. Select the `TaskerLauncherShortcut` action and search and select the desired shortcut by pressing the search icon in the plugin configuration activity. Then run the task. Make sure the intent `Uri` for the selected intent does not have the `com.android.launcher3.DEEP_SHORTCUT` category, otherwise shortcut will not work unless the TaskerLauncherShortcut app is the default launcher.
##


### Current Features

- Tasks to start `DEEP_SHORTCUT` and `Non DEEP_SHORTCUT` shortcuts
- Tasks to change default launcher using `root` or `ADB`
##


### Planned Features

- Change default launcher using Device owner APIs.
##


### Issues

`-`
##


### Worthy Of Note

`-`
##


### FAQs And FUQs

Check [FAQs_And_FUQs.md](FAQs_And_FUQs.md) file for the **Frequently Asked Questions(FAQs)** and **Frequently Unasked Questions(FUQs)**.
##


### Changelog

Check [CHANGELOG.md](CHANGELOG.md) file for the **Changelog**.
##


### Contributions

`-`
##


[Tasker App]: https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm
[TaskerLauncherShortcut App]: https://github.com/agnostic-apollo/TaskerLauncherShortcut
