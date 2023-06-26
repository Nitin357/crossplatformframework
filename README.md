<h1>Test Automation: Technical Guide</h1>

## Overview
The project will cover test automation of Newsreader apps for both iOS and Android (and soon Web), along with other projects. 
Test cases will be categorized according to the OS and further sub-categorized according to the functionalities they are testing.

<br><hr>

## Technical Specifications
The following is a list of all required software and tools used to run automation. 
Please be careful to use only the specified versions of these tools, as getting them wrong could lead to potential issues.

| Type     | Name                    | Current Version  |
|----------|-------------------------|------------------|
| Language | Python                  | 3.11.o           |
| IDE      | Android Studio          | Dolphin 2021.3.1 |
| IDE      | Pycharm(CE)             | 2022.3           |
| IDE      | XCode                   | 14.0             |
| VCS      | Git                     |                  |
| Driver   | Appium Desktop / Server | 1.22.3-4         |
| Other    | NPM                     | 8.19.2           | 
| Tools    | Maven Surefire Plugin   | 2.21.0           |
| Tools    | Maven Compiler Plugin   | 1.8              |
| Tools    | TestNG                  | 7.4.0            |

<br><hr>

## 1. Basic Environment Setup
1. Enable Super-User / Admin privileges on your Mac:
   1. This is **important** to be able to install anything on your machine
   2. In mac dock, Double-click "Privileges" app
   3. Click "Request privileges" button to activate super-user/admin mode rights<br>
   <br>
2. Download & Install JetBrains Toolbox
   * The JB Toolbox will be used to install and manage different versions of IDEs that we'll be using: 
   [JetBrains Toolbox](https://www.jetbrains.com/toolbox-app/)<br>
    <br>
3. Use the JB Toolbox to install:
   * Android Studio (by Google)
   * IntelliJ IDEA Community Edition<br>
   <br>

4. [Install Homebrew](https://docs.brew.sh/Installation)
   * In Terminal: 
   ```
   $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

5. [Install JAVA](https://devqa.io/brew-install-java/)
   * In Terminal:
   ```
   $ brew tap homebrew/cask-versions
   $ brew update
   $ brew tap homebrew/cask
   ```
   * To install our currently used version (if it differs from the latest available):
   ```
   $ brew tap adoptopenjdk/openjdk
   $ brew install adoptopenjdk19 --cask
   ```
   * Make sure that `adoptopenjdk<version>` lists the correct version referenced in "Technical Specifications" above.<br>
   <br>
6. [Install Appium Desktop Client](https://github.com/appium/appium-desktop/releases/latest)
   * Download our currently used version listed in "Technical Specifications" above.
   * In Finder > "Applications", double-click "Appium" app, and make sure it opens.

7. [Install Python Appium Client](https://pypi.org/project/Appium-Python-Client/)
   * Install the python appium client using Terminal:
   ```
   $ pip install Appium-Python-Client
   ```

8. [Install Appium terminal modules](https://appium.io/docs/en/about-appium/getting-started/?lang=en)
   * In Terminal (with admin "privileges" enabled):
   ```
   $ sudo npm install -g appium --unsafe-perm=true --allow-root
   $ sudo npm install wd --unsafe-perm=true --allow-root
   $ sudo npm install @appium/doctor --location=global --force
   $ npm install --location=global appium@next
   $ appium driver install xcuitest
   $ appium driver install uiautomator2
   $ appium server --base-path=/wd/hub
   ```
<br><hr>

## [1.1] Gatekeeper Installation Problems
If any installed application fails to open due to gatekeeper access rights, use the following hack to circumvent the problem:<br>
1. Open a Terminal window:
2. Execute: `xattr -cr </path/to/Application>.app` with the correct path entered for that app.
3. Open Finder window
4. Go to "Applications"
5. Right-click on application
6. Click "Open"
   
<br><hr>

## [2] Setup Access & Permissions
1. Request for access to NYTM and NYTIMES Github orgs through BYTES
   * https://nytimes.service-now.com/bytes?id=sc_cat_item&sys_id=14fe269d0f50c200551f5d78a1050e15
2. After you're added to the orgs:
   * Go to Github ["Settings"](https://github.com/nytimes/).
   * "Developer Settings" > "Personal Access Tokens" > "Tokens (classic)" > "Generate new token (classic)"
3. In Edit Personal Token, check off the Repo scope and hit "Update Token"
4. After the Token is generated, click on "Configure SSO" and Authorize nytimes and nytm
5. In Terminal:
   * `git clone https://github.com/nytimes/equ-automation.git` 
   * Use your Personal Access Token in place of the password


## [3] Setup First Test Run
1. Open Intellij IDEA
2. Open the "qa-app-automation" project
3. Click on Toolbar "Run" > “Edit Configurations...”
4. Select “TestNG”
5. Select "Add new run configuration"
6. Give the configuration a name ("Android-All" or "iOS-All" respectively)
7. Select "Configuration" tab
8. For "Test kind" select "Suite"
9. For "Suite" select an XML config file (/nytimes/qa-app-automation/src/test/java/<platform>_testng.xml)
10. Click "Apply" & "OK"
11. In the top toolbar, next to the run button, select the TestNG configuration you just created.
12. Click "Run" button
13. You can unselect specific test cases you don't want to run, by commenting-out their lines in the XML file.

   ### Report Document
   The detailed report will be found in html format after the execution of test cases at the location where the project is kept.

   ### Additonal Documentation
   https://docs.google.com/document/d/18rRjlVFd4GDLKT_3uH0SuBqWv3w-eKCtcCoR3CS-8LM/edit#heading=h.l74k59ntjlcr


<br><hr>


## [4.A] Android Setup
1. Open "Android Studio" via JB Toolbox installation.
2. Follow the default installation guide, and complete setup.
3. In a Finder window, open your Home/User folder (/Users/<user>)
4. _If it doesn't already exist, create a `.bash_profile`_
   1. _In Terminal:_
   ```
   cd /Users/<user>
   touch .bash_profile
   ```
5. Open your `.bash_profile` in a text editor, and make sure you have the following export keys added:
   ```
   export JAVA_HOME=<path_to_your_java_home_dir>
   export PATH=$JAVA_HOME/bin:$PATH
   export ANDROID_HOME=<path_to_your_android_sdk_directory>
   export PATH=$ANDROID_HOME/platform-tools:$PATH
   export PATH=$ANDROID_HOME/build-tools/<your_version>:$PATH
   export PATH=$ANDROID_HOME/cmdline-tools/latest/bin:$PATH
   export PATH=$ANDROID_HOME/emulator:$PATH
   ```

<br><hr>

## [4.B] iOS Setup

1. Build NYT Fusion app on Xcode(https://github.com/nytimes/ios-newsreader-fusion). 
Follow that README's "Required Setup" (Steps 1-7).
2. Build the app from Xcode to a simulator.
3. Install [WebDriverAgent](https://github.com/appium/appium-xcuitest-driver/blob/master/docs/real-device-config.md)
   * Follow the "Basic (automatic) configuration" if possible, or the "Basic (manual) configuration" if necessary.
   * The other configurations are not desirable, and probably not necessary.
4. Install `libimobiledevice`, an open source package which is able to communicate with iOS devices.
   * These applications weren't built to easily allow programmatic use, which is why Appium uses libimobiledevice for certain operations. 
   * `brew install libimobiledevice`
5. Install `ios-deploy`
   * Appium also uses a package called `ios-deploy` for transferring iOS apps onto your device, so let's install that too. 
   * `brew install ios-deploy`
6. Install `carthage`
   * WDA requires an iOS dependency manager called `carthage`. 
   * Since Appium will be automatically building the WDA app, we need to install Carthage so it is available to the WDA bootstrap process. 
   * `brew install carthage`
7. Once you complete the "Appium Setup" you should be ready to run some tests!

<br><hr>


## [5] Appium Setup
1. Steps to complete the setup of your Appium Environment:
   1. Appium Doctor (verifying a complete setup installation)
   2. Appium Server (running tests)
   3. Appium Inspector (debugging and inspecting applications)
2. You should by now have successfully installed Appium on your machine.
3. From a Finder window, open "Applications" > "Appium".
   * Click on "Edit Configurations", and add your `ANDROID_HOME` and `JAVA_HOME` paths.
   * Click "Save and Restart".
4. In Terminal, run `appium-doctor`.
5. Verify that you get all green checkmarks for the required components, and fix any that are red X's.
6. From a Finder window, open "Applications" > "Appium Inspector".
   * Set the "Remote Path" to `/wd/hub`
   * Under "Advanced Settings" > Click to turn on "Allow Unauthorized Certificates"
   * Click on "Desired Capabilities", and add the following capability sets for your Android and/or iOS Emulator/Simulator.
   * Pay closes attention to put in your own paths or names in places where you see `<carrated>` special designations.
   * Check to make sure "Automatically add necessary Appium vendor prefixes on start" is turned on.
   * Save your configurations, so you can start up an Android or iOS session using the Inspector any time.

### Example Android Emulator Capability Set  [Appium Inspector]
```
{
  "platformName": "Android",
  "appium:automationName": "UiAutomator2",
  "appium:deviceName": "Emulator",
  "appium:noReset": true,
  "appium:appWaitForLaunch": false,
  "appium:appPackage": "com.nytimes.android",
  "appium:platformVersion": "13",
  "appium:app": "<path_to_apk>/reader-universal-alpha.apk",
  "appium:udid": "<udid_of_device>"
}
```
### Example iOS Simulator Capability Set  [Appium Inspector]
```
{
  "platformName": "iOS",
  "appium:platformVersion": "16.0",
  "appium:deviceName": "Simulator",
  "appium:udid": "<udid_of_device>",
  "appium:bundleId": "com.nytimes.NYTimes.debug",
  "appium:xcodeOrgId": "FL458598BU",
  "appium:xcodeSigningId": "iPhone Developer",
  "appium:automationName": "XCUITest",
  "appium:useNewWDA": true,
  "appium:MobileCapabilityType.APP": "<path_to_your_app>",
  "appium:updatedWDABundleId": "com.test.webdriveragent",
  "appium:showXcodeLog": false,
  "wda:derivedDataPath": <path_to_your_WDA>
}
```
* **<path_to_your_app>** should look similar to this:
  * `"/Users/<user>/Library/Developer/Xcode/DerivedData/Newsreader-exulbberurxkwxgjftpxoigttlpz/Build/Products/Debug-iphonesimulator/NYTimes.app"`
* **<path_to_your_WDA>** should look similar to this:
  * `"/Users/<user>/Library/Developer/Xcode/DerivedData/WebDriverAgent-ciegwgvxzxdrqthilmrmczmqvrgu"`


<br><hr>

## That's It!

This document relies on everyone's participation to make sure it remains a living document.
If you find that there are any missing or changed steps not present in this document, please update them asap. 
Or reach out to someone who can update them for you.
