# Google Maps Setup Guide for Daily Milk Agent App

## Overview
This guide will help you set up Google Maps integration for the Daily Milk Agent application, enabling real-time location tracking, customer location display, and route calculation features.

## Prerequisites
1. Google Cloud Platform account
2. Google Maps Platform API access
3. Android development environment

## Step 1: Enable Google Maps APIs

### 1.1 Create a Google Cloud Project
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Note your Project ID

### 1.2 Enable Required APIs
Enable the following APIs in your Google Cloud Console:
- **Maps SDK for Android**
- **Places API**
- **Geocoding API**
- **Directions API**
- **Distance Matrix API**

To enable APIs:
1. Go to "APIs & Services" > "Library"
2. Search for each API and click "Enable"

## Step 2: Create API Keys

### 2.1 Create Android API Key
1. Go to "APIs & Services" > "Credentials"
2. Click "Create Credentials" > "API Key"
3. Copy the generated API key
4. Click on the API key to configure restrictions

### 2.2 Restrict API Key (Recommended)
1. Under "Application restrictions", select "Android apps"
2. Add your package name: `com.example.dailymilk_agent`
3. Add your SHA-1 certificate fingerprint:
   ```bash
   # For debug builds
   keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
   
   # For release builds (replace with your keystore path)
   keytool -list -v -keystore /path/to/your/release.keystore -alias your-alias
   ```
4. Under "API restrictions", select "Restrict key" and choose the APIs you enabled

## Step 3: Configure Android Application

### 3.1 Update AndroidManifest.xml
Replace `YOUR_GOOGLE_MAPS_API_KEY_HERE` in `android/app/src/main/AndroidManifest.xml` with your actual API key:

```xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="AIzaSyC_YOUR_ACTUAL_API_KEY_HERE" />
```

### 3.2 Update Dependencies
Run the following command to install the new dependencies:
```bash
flutter pub get
```

## Step 4: Test the Setup

### 4.1 Build and Run
```bash
# Clean build
flutter clean
flutter pub get

# Run the app
flutter run
```

### 4.2 Test Maps Functionality
1. Navigate to the Maps tab in the app
2. Grant location permissions when prompted
3. Verify that:
   - Your current location appears on the map
   - Customer locations are displayed as markers
   - Tapping on markers shows order details
   - Route calculation works between locations

## Step 5: Production Setup

### 5.1 Generate Release Keystore
```bash
keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
```

### 5.2 Configure Signing
Update `android/app/build.gradle` with your keystore details:
```gradle
android {
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
            storePassword keystoreProperties['storePassword']
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
    }
}
```

### 5.3 Update API Key Restrictions
1. Get your release SHA-1 fingerprint
2. Add it to your API key restrictions in Google Cloud Console
3. Test with a release build

## Features Implemented

### ✅ Core Maps Features
- Real-time GPS location tracking
- Google Maps integration with custom markers
- Customer location display from Firebase
- Agent current location marker
- Location permissions handling

### ✅ Delivery Management
- Order details popup on marker tap
- Route visualization between agent and customer
- Distance and estimated time calculation
- Mark orders as delivered functionality
- Order status updates in Firebase

### ✅ Navigation Features
- Center map on current location
- Show all orders on map
- Zoom to fit all delivery points
- Route drawing with polylines

### ✅ UI/UX Features
- Bottom navigation integration
- Floating action buttons for quick actions
- Loading states and error handling
- Permission request dialogs
- Order details bottom sheet

## Troubleshooting

### Common Issues

1. **Maps not loading**
   - Check API key is correctly set
   - Verify APIs are enabled in Google Cloud Console
   - Check internet connectivity

2. **Location not working**
   - Ensure location permissions are granted
   - Check GPS is enabled on device
   - Verify location services are enabled in app settings

3. **Markers not showing**
   - Check Firebase data has latitude/longitude fields
   - Verify orders have non-null coordinate values
   - Check Firebase security rules allow reading

4. **Build errors**
   - Run `flutter clean && flutter pub get`
   - Check all dependencies are properly installed
   - Verify Android SDK is up to date

### Debug Commands
```bash
# Check dependencies
flutter doctor

# Verbose build output
flutter run --verbose

# Check Firebase connection
flutter run --debug
```

## Security Notes

1. **API Key Security**
   - Always restrict API keys to specific apps and APIs
   - Use different keys for development and production
   - Monitor API usage in Google Cloud Console

2. **Location Privacy**
   - Request location permissions appropriately
   - Inform users about location usage
   - Follow platform guidelines for location handling

3. **Firebase Security**
   - Configure proper security rules
   - Limit data access to authenticated users
   - Use proper indexing for location queries

## Next Steps

Consider implementing these additional features:
- Turn-by-turn navigation integration
- Offline maps support
- Location history tracking
- Geofencing for delivery confirmations
- Push notifications for nearby deliveries
- Route optimization for multiple deliveries

## Support

For issues related to:
- Google Maps: [Google Maps Platform Support](https://developers.google.com/maps/support)
- Flutter: [Flutter Documentation](https://docs.flutter.dev/)
- Firebase: [Firebase Documentation](https://firebase.google.com/docs)

## Cost Considerations

Monitor your Google Maps API usage to avoid unexpected charges:
- Set up billing alerts
- Use API quotas and limits
- Consider caching strategies for frequently accessed data
- Review pricing at [Google Maps Pricing](https://developers.google.com/maps/pricing) 