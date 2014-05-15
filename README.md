# Chrome Apps Developer Tool for Mobile

The Chrome Apps Developer Tool ("Chrome ADT" for short) is a distribution of
[Apache Cordova App Harness](https://git-wip-us.apache.org/repos/asf/cordova-app-harness.git)
that can run Chrome Apps. It is based on the plugins from the
[cca](https://github.com/MobileChomeApps/mobile-chrome-apps) toolkit.

## Use a Pre-built APK
Pre-built APKs are available [here](https://github.com/MobileChromeApps/harness/releases).

## Updating from cordova-app-harness

    git checkout upstream
    git pull /path/to/cordova-app-harness master
    git checkout master
    git merge upstream
    git push origin master upstream

## Creating a Project
Use `createproject.sh` to create a project. Example invocation:

    PLATFORMS="android ios" ./createproject.sh NewProject

For more info:

    ./createproject.sh --help

### Extra Steps

- Replace the default Cordova icons with your desired icons.
  - `rm platforms/android/res/drawable-*/icon.png`
  - `cp ../../mobile-chrome-apps/templates/default-app/assets/icons/icon128.png platforms/android/res/drawable/icon.png`
- Replace the title in index.html to "Chrome ADT vX.X.X"
  - `vim www/cdvah/index.html`

### Cutting a Release

- Update the version in `config.xml`
  - `vim config.xml`
-Build apk
  - `cd CCAHarness && ../buildharness.sh`
- Tag release
  - `git tag -am "Tagged vX.X.X-alpha" vX.X.X-alpha`
  - `git push origin master --tags`
- Upload apk to GitHub's releases page
  - Attach the apk
  - Write *short* release notes (download link should be visible without scrolling).

## Features
* Install and launch via `cordova serve`
* Three-finger swipe to return to main menu

## Major Unimplemented Features
* Applying app settings (DisallowOverscroll, etc)
* Applying app splashscreen
* Applying app's whitelist
* Enabling only the plugins that the app has installed

## Major Unimplemented In-App Menu Features
* Inject a JSConsole script tag
* JSHybugger support
* Simulator-friendly menu gesture (shake gesture?)

## Test by using `cordova serve`
* Go to Cordova project of the app you want to test in a terminal and run.

      cordova serve <platform>

  * If you are running this on a simulator, you can use `http://localhost` as your address, or on Android `10.0.0.22`.
  * If `cordova serve` is on a different network than your App Harness, then use [ProxyLocal](http://proxylocal.com/) or [LocalTunnel](http://progrium.com/localtunnel/) to forward the port.

## Running an app in the harness
* Click launch on the installed app
* See if the app looks as expected
* Use a 3 finger swipe to access the app menu while testing your app.
* The context menu that pops up allows you to return to the main screen, restart or update the app, open a Firebug console on the device, or set up remote debugging using Weinre.

# Harness Server

A server runs within the app that enables remote control functionality.

## Port Forwarding (Android)

If you are not on the same network, you can use adb to port forward:

    adb forward tcp:2424 tcp:2424

## Commands

### /push

Add or update an app's settings, and then update & launch:

    curl -X POST http://$IP_ADDRESS:2424/push?type=serve&name=com.example.YourApp&url=http://$SERVE_HOST_ADDRESS:8000


### /menu

Return to main menu:

    curl -X POST http://$IP_ADDRESS:2424/menu

### /exec

Executes a JS snippet:

    curl -X POST http://$IP_ADDRESS:2424/exec?code='alert(1)'

### /info

Returns JSON of server info / app state

    curl http://$IP_ADDRESS:2424/info

