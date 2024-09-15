# Turn-by-Turn Navigation with Mapbox in Flutter

This project demonstrates how to integrate Mapbox's turn-by-turn navigation in a Flutter application. Follow the instructions below to set up Mapbox for both iOS and Android platforms.

## Prerequisites

- Flutter SDK
- Mapbox account and access token

## Getting Started

### 1. Add Mapbox Dependencies

In your `pubspec.yaml` file, add the Mapbox dependencies:

```yaml
dependencies:
  flutter:
    sdk: flutter
  mapbox_gl: ^1.0.0  # or the latest version
  flutter_mapbox_navigation: ^0.0.1  # or the latest version
```
Run the following command to install the packages:

```
flutter pub get
```
## 2. iOS Configuration
 a. Update Info.plist
Open ios/Runner/Info.plist in your iOS project directory.

Add the following entries:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>We need your location to provide turn-by-turn navigation.</string>
<key>NSLocationAlwaysUsageDescription</key>
<string>We need your location to provide turn-by-turn navigation.</string>
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
<key>MapboxAccessToken</key>
<string>YOUR_MAPBOX_ACCESS_TOKEN</string>
```
Replace YOUR_MAPBOX_ACCESS_TOKEN with your actual Mapbox access token.

b. Update AppDelegate.swift
Open ios/Runner/AppDelegate.swift.

Add the following import and initialization code:
```swift
import Mapbox

@UIApplicationMain
class AppDelegate: FlutterAppDelegate {
  override func application(_ application: UIApplication,
                            didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    MGLAccountManager.accessToken = "YOUR_MAPBOX_ACCESS_TOKEN"
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```
Replace YOUR_MAPBOX_ACCESS_TOKEN with your actual Mapbox access token.

### 3. Android Configuration
a. Update AndroidManifest.xml
Open android/app/src/main/AndroidManifest.xml.

Add the following permissions and metadata:
```
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<application
    ... >
    <!-- Add this inside the application tag -->
    <meta-data
        android:name="com.mapbox.token"
        android:value="YOUR_MAPBOX_ACCESS_TOKEN" />
    ...
</application>
```
Replace YOUR_MAPBOX_ACCESS_TOKEN with your actual Mapbox access token.

b. Add mapbox Dependency to build.gradle
Open android/build.gradle and ensure you have the Mapbox repository:
```
allprojects {
    repositories {
        google()
        mavenCentral()
        maven { url 'https://mapbox.bintray.com/mapbox' }
    }
}
```
Open android/app/build.gradle and add the Mapbox dependency:
```
dependencies {
    implementation 'com.mapbox.mapboxsdk:mapbox-sdk-services:5.6.0' // or the latest version
}
```
## 4. Setting Up Navigation
Here's an example of how to set up Mapbox navigation in your Flutter app:
```
import 'package:flutter/material.dart';
import 'package:flutter_mapbox_navigation/flutter_mapbox_navigation.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MapScreen(),
    );
  }
}

class MapScreen extends StatefulWidget {
  @override
  _MapScreenState createState() => _MapScreenState();
}

class _MapScreenState extends State<MapScreen> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Mapbox Navigation'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () async {
            final directions = MapboxDirections(
              accessToken: 'YOUR_MAPBOX_ACCESS_TOKEN',
              origin: LatLng(37.7749, -122.4194), // San Francisco
              destination: LatLng(34.0522, -118.2437), // Los Angeles
            );
            await directions.startNavigation();
          },
          child: Text('Start Navigation'),
        ),
      ),
    );
  }
}
```
Replace YOUR_MAPBOX_ACCESS_TOKEN with your actual token.

#### Notes
### Make sure to keep your Mapbox access token secure and do not expose it in public repositories.
### Check the official Mapbox documentation for any updates or changes in the integration process.
