# Guide-Flutter-PopOS
A guide on how I was able to make flutter doctor work on my pop os with Flatpak Android Studio

## My Setup
I have:
- Android Studio as Flatpak already installed
- I installed Flutter using snap
- I'm on Pop_OS

## Issues
After running `$ flutter doctor` I get the following:
```bash
Running "flutter pub get" in flutter_tools...                    2,601ms
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 3.0.2, on Pop!_OS 21.10 5.17.11-xanmod1, locale en_US.UTF-8)
[✗] Android toolchain - develop for Android devices
    ✗ cmdline-tools component is missing
      Run `path/to/sdkmanager --install "cmdline-tools;latest"`
      See https://developer.android.com/studio/command-line for more details.
[✗] Chrome - develop for the web (Cannot find Chrome executable at google-chrome)
    ! Cannot find Chrome. Try setting CHROME_EXECUTABLE to a Chrome executable.
[✓] Linux toolchain - develop for Linux desktop
[!] Android Studio (not installed)
[✓] VS Code (version 1.67.2)
[✓] Connected device (1 available)
[✓] HTTP Host Availability

! Doctor found issues in 3 categories.
```
And for `$ flutter doctor -v`:
```bash
[✓] Flutter (Channel stable, 3.0.2, on Pop!_OS 21.10 5.17.11-xanmod1, locale en_US.UTF-8)
    • Flutter version 3.0.2 at /home/hazem/snap/flutter/common/flutter
    • Upstream repository https://github.com/flutter/flutter.git
    • Framework revision cd41fdd495 (4 days ago), 2022-06-08 09:52:13 -0700
    • Engine revision f15f824b57
    • Dart version 2.17.3
    • DevTools version 2.12.2

[✗] Android toolchain - develop for Android devices
    • Android SDK at /usr/lib/android-sdk
    ✗ cmdline-tools component is missing
      Run `path/to/sdkmanager --install "cmdline-tools;latest"`
      See https://developer.android.com/studio/command-line for more details.

[✗] Chrome - develop for the web (Cannot find Chrome executable at google-chrome)
    ! Cannot find Chrome. Try setting CHROME_EXECUTABLE to a Chrome executable.

[✓] Linux toolchain - develop for Linux desktop
    • clang version 6.0.0-1ubuntu2 (tags/RELEASE_600/final)
    • cmake version 3.10.2
    • ninja version 1.8.2
    • pkg-config version 0.29.1

[!] Android Studio (not installed)
    • Android Studio not found; download from https://developer.android.com/studio/index.html
      (or visit https://flutter.dev/docs/get-started/install/linux#android-setup for detailed instructions).

[✓] VS Code (version 1.67.2)
    • VS Code at /usr/share/code
    • Flutter extension can be installed from:
      🔨 https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter

[✓] Connected device (1 available)
    • Linux (desktop) • linux • linux-x64 • Pop!_OS 21.10 5.17.11-xanmod1

[✓] HTTP Host Availability
    • All required HTTP hosts are available

! Doctor found issues in 3 categories.
```

## My Fixes
NOTICE: THESE ARE MY FIXES, THEY MIGHT NOT APPLY TO YOU EXACTLY THE SAME AND THIS IS INTENTED TO HELP YOU FIGURE OUT THE PATTERNS. GOOD LUCK!

- For `Android Studio (not installed)` the solution was `$ flutter config --android-studio-dir=/home/hazem/.local/share/flatpak/app/com.google.AndroidStudio/x86_64/stable/active/files/extra/android-studio`
```bash
[✓] Android Studio
    • Android Studio at /home/hazem/.local/share/flatpak/app/com.google.AndroidStudio/x86_64/stable/active/files/extra/android-studio
    • Flutter plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/9212-flutter
    • Dart plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/6351-dart
    • android-studio-dir = /home/hazem/.local/share/flatpak/app/com.google.AndroidStudio/x86_64/stable/active/files/extra/android-studio
    • Java version OpenJDK Runtime Environment (build 11.0.12+0-b1504.28-7817840)
```

- For the Android SDK, even though the log reports no issues with it, I was able to locate the correct path from the Android Studio Tools > SDK Manager
    ![image](https://user-images.githubusercontent.com/94578938/173247131-3bec6d1a-577f-4fc5-8079-7df780b7e684.png)
    So I ran `$ flutter config --android-sdk=/home/hazem/Storage/Android/Sdk`

- For `✗ cmdline-tools component is missing`, I downloaded *Command line tools only* from https://developer.android.com/studio#downloads Then I extracted the archive to downloads, went into the dir where `sdkmanager` executable is and ran this command to install cmdline-tools to the SDK dir from above `$ ./sdkmanager --sdk_root=/home/hazem/Storage/Android/Sdk "cmdline-tools;latest"`
-  Eventually you will have to run `$ flutter doctor --android-licenses` and type `y` and hit enter to accept licenses

## Result

`$ flutter doctor -v`:
```bash
[✓] Flutter (Channel stable, 3.0.2, on Pop!_OS 21.10 5.17.11-xanmod1, locale en_US.UTF-8)
    • Flutter version 3.0.2 at /home/hazem/snap/flutter/common/flutter
    • Upstream repository https://github.com/flutter/flutter.git
    • Framework revision cd41fdd495 (4 days ago), 2022-06-08 09:52:13 -0700
    • Engine revision f15f824b57
    • Dart version 2.17.3
    • DevTools version 2.12.2

[✓] Android toolchain - develop for Android devices (Android SDK version 32.1.0-rc1)
    • Android SDK at /home/hazem/Storage/Android/Sdk
    • Platform android-32, build-tools 32.1.0-rc1
    • Java binary at:
      /home/hazem/.local/share/flatpak/app/com.google.AndroidStudio/x86_64/stable/active/files/extra/android-studio/jre/bin/java
    • Java version OpenJDK Runtime Environment (build 11.0.12+0-b1504.28-7817840)
    • All Android licenses accepted.

[✗] Chrome - develop for the web (Cannot find Chrome executable at google-chrome)
    ! Cannot find Chrome. Try setting CHROME_EXECUTABLE to a Chrome executable.

[✓] Linux toolchain - develop for Linux desktop
    • clang version 6.0.0-1ubuntu2 (tags/RELEASE_600/final)
    • cmake version 3.10.2
    • ninja version 1.8.2
    • pkg-config version 0.29.1

[✓] Android Studio
    • Android Studio at /home/hazem/.local/share/flatpak/app/com.google.AndroidStudio/x86_64/stable/active/files/extra/android-studio
    • Flutter plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/9212-flutter
    • Dart plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/6351-dart
    • android-studio-dir = /home/hazem/.local/share/flatpak/app/com.google.AndroidStudio/x86_64/stable/active/files/extra/android-studio
    • Java version OpenJDK Runtime Environment (build 11.0.12+0-b1504.28-7817840)

[✓] VS Code (version 1.67.2)
    • VS Code at /usr/share/code
    • Flutter extension can be installed from:
      🔨 https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter

[✓] Connected device (1 available)
    • Linux (desktop) • linux • linux-x64 • Pop!_OS 21.10 5.17.11-xanmod1

[✓] HTTP Host Availability
    • All required HTTP hosts are available

! Doctor found issues in 1 category.
```
I am missing chrome since I don't have it and won't use flutter to develop for web (as of writing this)

## Contributing
Feel free to open a PR.

## Technical Support
I **MIGHT** help you out with your issue if you open a new issue and support logs and information that will help me assist you. I am doing this just because I like to help others, but by no mean this is a promise I will be able to solve your issue or help you.
Good Luck and hope I helped you today, enjoy your journey!

## Donate/Support Me
You can donate to this paypal `ritaanimax@gmail.com` and I will appreciate it a lot ❤️
