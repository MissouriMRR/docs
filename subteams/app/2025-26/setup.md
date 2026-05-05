# Setup

## Environment

- [Install Flutter](https://docs.flutter.dev/install/custom)
- [Add Flutter to PATH](https://docs.flutter.dev/install/add-to-path)

## Pushing Code to Phone

Uses [ADB (Android Debug Bridge)](https://developer.android.com/tools/adb).

1. Pair your device:
   ```
   adb pair <PHONE_IP>:<PAIRING_PORT>
   ```
   > **Note:** This is not the IP/port shown on the wireless debugging screen. Tap "Pair with code" to get a separate pairing port.

2. Connect to the device:
   ```
   adb connect <PHONE_IP>:<PORT>
   ```

3. Install the APK:
   ```
   adb install /path/to/apk
   ```
    > **Note:** on my computer the path was build/app/outputs/flutter-apk/app-release.apk 
