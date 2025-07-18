# BoltFit - Fitness Tracking App

A comprehensive fitness tracking app with gamification, AI-powered workouts, and health platform integration.

## Setup Instructions

### Prerequisites

- Node.js 16 or higher
- Yarn or npm
- Xcode (for iOS development)
- Android Studio (for Android development)
- Google Cloud account
- Firebase account

### API Keys and Configuration

1. **Firebase Setup**

   - Create a new Firebase project
   - Enable Authentication, Firestore, and Analytics
   - Download `google-services.json` (Android) and place in `packages/mobile/android/app/`
   - Download `GoogleService-Info.plist` (iOS) and place in `packages/mobile/ios/`
   - Add Firebase configuration to `packages/mobile/src/config/env.ts`

2. **Google Fit (Android)**

   - Go to Google Cloud Console
   - Create a new project or select existing project
   - Enable the Fitness API
   - Create OAuth 2.0 client credentials
   - Add client ID and secret to `packages/mobile/src/config/env.ts`

3. **Apple HealthKit (iOS)**

   - Open Xcode project
   - Enable HealthKit capability in project settings
   - Add required privacy descriptions to Info.plist:
     ```xml
     <key>NSHealthShareUsageDescription</key>
     <string>BoltFit needs access to your health data to track your workouts and fitness metrics.</string>
     <key>NSHealthUpdateUsageDescription</key>
     <string>BoltFit needs permission to save your workouts and fitness data.</string>
     ```

4. **Gemini AI API**
   - Go to Google Cloud Console
   - Enable the Gemini API
   - Create an API key
   - Add the API key to `packages/mobile/src/config/env.ts`

### Installation

1. Install dependencies:

   ```bash
   yarn install
   ```

2. Install iOS dependencies:

   ```bash
   cd packages/mobile/ios
   pod install
   ```

3. Create a `.env` file in `packages/mobile/` with your API keys (use `.env.example` as template)

4. Update app configuration in `packages/mobile/src/config/env.ts`

### Development

1. Start Metro bundler:

   ```bash
   yarn start
   ```

2. Run on iOS:

   ```bash
   yarn ios
   ```

3. Run on Android:
   ```bash
   yarn android
   ```

## App Store Deployment

### iOS App Store

1. Configure app signing in Xcode:

   - Open Xcode project
   - Select your team
   - Configure provisioning profiles
   - Set bundle identifier

2. Create app in App Store Connect:

   - Log in to App Store Connect
   - Create new app
   - Configure app information
   - Add required screenshots and metadata

3. Archive and upload:
   - In Xcode, select "Any iOS Device" as build target
   - Select Product > Archive
   - Upload to App Store using Xcode organizer

### Google Play Store

1. Configure app signing:

   - Generate upload keystore:
     ```bash
     keytool -genkey -v -keystore upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
     ```
   - Add keystore to `packages/mobile/android/app/`
   - Update `android/app/build.gradle` with signing config

2. Create app in Google Play Console:

   - Log in to Google Play Console
   - Create new app
   - Configure app information
   - Add required screenshots and metadata

3. Build and upload:
   ```bash
   cd packages/mobile/android
   ./gradlew bundleRelease
   ```
   - Upload the generated AAB file to Google Play Console

## Environment Variables

Required environment variables for production deployment:

```bash
# Firebase
FIREBASE_API_KEY=
FIREBASE_APP_ID=
FIREBASE_MEASUREMENT_ID=
FIREBASE_PROJECT_ID=
FIREBASE_AUTH_DOMAIN=
FIREBASE_STORAGE_BUCKET=
FIREBASE_MESSAGING_SENDER_ID=

# Google Fit
GOOGLE_FIT_CLIENT_ID=
GOOGLE_FIT_CLIENT_SECRET=

# Gemini AI
GEMINI_API_KEY=

# App Configuration
APP_ENVIRONMENT=production
```

## Health Platform Integration

### iOS HealthKit

Required Info.plist entries:

```xml
<key>NSHealthShareUsageDescription</key>
<string>BoltFit needs access to your health data to track your workouts and fitness metrics.</string>
<key>NSHealthUpdateUsageDescription</key>
<string>BoltFit needs permission to save your workouts and fitness data.</string>
```

### Google Fit

Required Android Manifest permissions:

```xml
<uses-permission android:name="android.permission.ACTIVITY_RECOGNITION"/>
<uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION"/>
```

## Support

For technical support or questions about API integration, please contact the development team.
