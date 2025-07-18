# API Keys Setup Guide for BoltFit

## 1. Firebase Setup

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project or select existing
3. Add apps (iOS and Android):
   - iOS: Bundle ID = `com.yourdomain.boltfit`
   - Android: Package name = `com.yourdomain.boltfit`
4. Download config files:
   - `google-services.json` → `packages/mobile/android/app/`
   - `GoogleService-Info.plist` → `packages/mobile/ios/`
5. Get configuration values from project settings:

```bash
FIREBASE_API_KEY=           # Found in Firebase Console > Project Settings > General > Web API Key
FIREBASE_APP_ID=           # Found in Firebase Console > Project Settings > General > Your Apps
FIREBASE_PROJECT_ID=       # Found in Firebase Console > Project Settings > General
FIREBASE_AUTH_DOMAIN=      # Format: your-project-id.firebaseapp.com
FIREBASE_STORAGE_BUCKET=   # Format: your-project-id.appspot.com
FIREBASE_MESSAGING_SENDER_ID= # Found in Firebase Console > Project Settings > Cloud Messaging
```

## 2. Google Cloud Setup (for Google Fit & Gemini AI)

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing
3. Enable required APIs:
   - Fitness API
   - Gemini API

### Google Fit Setup

1. Go to APIs & Services > Credentials
2. Create OAuth 2.0 Client ID:
   - Application type: Android
   - Package name: `com.yourdomain.boltfit`
   - SHA-1 signing certificate: Get from your keystore

```bash
GOOGLE_FIT_CLIENT_ID=      # OAuth 2.0 Client ID
GOOGLE_FIT_CLIENT_SECRET=  # OAuth 2.0 Client Secret
```

### Gemini AI Setup

1. Go to APIs & Services > Credentials
2. Create API Key:
   - Click "Create Credentials" > "API Key"
   - Restrict the key to Gemini API only

```bash
GEMINI_API_KEY=           # Your Gemini API Key
```

## 3. Apple Developer Setup (for HealthKit)

1. Go to [Apple Developer Portal](https://developer.apple.com/)
2. Enable HealthKit capability:
   - In Xcode: Signing & Capabilities > + Capability > HealthKit
3. Add to Info.plist:

```xml
<key>NSHealthShareUsageDescription</key>
<string>BoltFit needs access to your health data to track your workouts and fitness metrics.</string>
<key>NSHealthUpdateUsageDescription</key>
<string>BoltFit needs permission to save your workouts and fitness data.</string>
```

## 4. Creating the Environment Files

### 1. Create `.env` file in `packages/mobile/`:

```bash
# Firebase Configuration
FIREBASE_API_KEY=your_firebase_api_key
FIREBASE_APP_ID=your_firebase_app_id
FIREBASE_PROJECT_ID=your_firebase_project_id
FIREBASE_AUTH_DOMAIN=your_firebase_auth_domain
FIREBASE_STORAGE_BUCKET=your_firebase_storage_bucket
FIREBASE_MESSAGING_SENDER_ID=your_firebase_messaging_sender_id

# Google Fit
GOOGLE_FIT_CLIENT_ID=your_google_fit_client_id
GOOGLE_FIT_CLIENT_SECRET=your_google_fit_client_secret

# Gemini AI
GEMINI_API_KEY=your_gemini_api_key

# App Configuration
APP_ENVIRONMENT=development
```

### 2. Update Android Manifest

Add to `packages/mobile/android/app/src/main/AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.ACTIVITY_RECOGNITION"/>
<uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION"/>
```

### 3. Update iOS Info.plist

Add to `packages/mobile/ios/BoltFit/Info.plist`:

```xml
<key>NSHealthShareUsageDescription</key>
<string>BoltFit needs access to your health data to track your workouts and fitness metrics.</string>
<key>NSHealthUpdateUsageDescription</key>
<string>BoltFit needs permission to save your workouts and fitness data.</string>
```

## 5. Verifying Setup

### Test Firebase:

```bash
# In packages/mobile
yarn start
# Then in another terminal
yarn ios  # or yarn android
```

Check Firebase Console > Analytics to verify app is sending data.

### Test Google Fit:

1. Run app on Android device
2. Try connecting to Google Fit
3. Verify permissions dialog appears

### Test HealthKit:

1. Run app on iOS device/simulator
2. Try accessing health data
3. Verify permissions dialog appears

### Test Gemini AI:

1. Try generating a workout
2. Check console for successful API response

## 6. Common Issues & Solutions

1. Firebase Connection Issues:

   - Verify SHA-1 fingerprint in Firebase Console
   - Check bundle ID/package name matches
   - Ensure config files are in correct locations

2. Google Fit Issues:

   - Verify OAuth 2.0 credentials
   - Check SHA-1 fingerprint matches
   - Ensure Fitness API is enabled

3. HealthKit Issues:

   - Verify capability is enabled in Xcode
   - Check Info.plist entries
   - Test on physical device

4. Gemini AI Issues:
   - Check API key restrictions
   - Verify API is enabled in Google Cloud Console
   - Check usage quotas

## 7. Security Best Practices

1. Never commit `.env` file to version control
2. Add API key restrictions in respective consoles
3. Use different API keys for development and production
4. Regularly rotate API keys
5. Monitor API usage and set up alerts

## 8. Production Deployment

Before deploying to app stores:

1. Create production Firebase project
2. Generate production API keys
3. Update environment variables
4. Test all integrations with production keys
5. Set up proper API key restrictions
6. Configure proper OAuth consent screens
