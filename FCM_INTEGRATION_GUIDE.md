# Firebase Cloud Messaging (FCM) Integration Guide

## Overview
This guide covers the complete integration of Firebase Cloud Messaging (FCM) in your Flutter app to receive push notifications when bookings are confirmed. The app subscribes to the `customer_Teja` topic and handles notifications in all app states.

## 🔧 Implementation Summary

### 1. Dependencies Added
- `firebase_messaging: ^14.7.10` - Core FCM functionality
- `flutter_local_notifications: ^17.0.0` - Local notification display

### 2. Files Created/Modified

#### New Files:
- `lib/services/fcm_service.dart` - Main FCM service class
- `lib/debug/fcm_test_screen.dart` - Debug screen for testing FCM
- `android/app/src/main/res/drawable/ic_notification.xml` - Notification icon
- `android/app/src/main/res/values/colors.xml` - Notification colors

#### Modified Files:
- `pubspec.yaml` - Added FCM dependencies
- `android/app/src/main/AndroidManifest.xml` - Added FCM permissions and services
- `lib/main.dart` - Initialize FCM service
- `lib/features/home/screens/home_screen.dart` - Added FCM test screen navigation

## 📱 Features Implemented

### Core FCM Features:
1. **Topic Subscription**: Automatically subscribes to `customer_Teja` topic
2. **Permission Handling**: Requests notification permissions on Android
3. **Token Management**: Generates, saves, and refreshes FCM tokens
4. **Message Handling**: Handles notifications in all app states:
   - **Foreground**: Shows local notifications
   - **Background**: Handles app opening from notification
   - **Terminated**: Handles app launch from notification

### Notification Features:
1. **Rich Notifications**: Displays booking details (ID, amount, date)
2. **Custom Styling**: Uses app colors and custom notification icon
3. **Sound & Vibration**: Enabled for important notifications
4. **Notification Channel**: Dedicated channel for booking notifications

## 🚀 How to Use

### 1. Testing the Integration

#### Access FCM Test Screen:
1. Run the app
2. Go to home screen
3. **Long press** the notification icon in the top bar
4. This opens the FCM Test Screen

#### In FCM Test Screen:
1. **Copy FCM Token**: Copy the device token for direct messaging
2. **Check Permissions**: Verify notification permissions are granted
3. **Subscribe/Unsubscribe**: Manage topic subscription
4. **Refresh Token**: Generate new FCM token if needed

### 2. Sending Test Notifications

#### Option A: Firebase Console
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Select your project
3. Navigate to **Cloud Messaging**
4. Click **"Send your first message"**
5. Configure message:
   ```
   Title: Booking Confirmed!
   Body: Your order has been confirmed and will be delivered soon.
   
   Target: Topic
   Topic: customer_Teja
   
   Additional options > Custom data:
   Key: type, Value: booking_confirmation
   Key: bookingId, Value: BK001
   Key: customerName, Value: John Doe
   Key: totalAmount, Value: 250
   Key: orderDate, Value: 2024-01-15
   ```

#### Option B: Direct Token Messaging
Use the copied FCM token to send direct messages via Firebase Console or FCM API.

### 3. Expected Notification Behavior

#### When App is in Foreground:
- Local notification appears at the top
- Shows booking details in expanded view
- Plays sound and vibrates

#### When App is in Background:
- System notification appears
- Tapping opens the app
- Logs message details in debug console

#### When App is Terminated:
- System notification appears
- Tapping launches the app
- Handles initial message on app startup

## 🔑 Key Components

### FCMService Class
Located in `lib/services/fcm_service.dart`

**Main Methods:**
- `initialize()` - Sets up FCM, requests permissions, subscribes to topic
- `subscribeToTopic(topic)` - Subscribe to specific topic
- `getCurrentToken()` - Get current FCM token
- `areNotificationsEnabled()` - Check permission status

### Message Data Structure
Expected data payload for booking notifications:
```json
{
  "type": "booking_confirmation",
  "bookingId": "BK001",
  "customerName": "Customer Name",
  "totalAmount": "250",
  "orderDate": "2024-01-15"
}
```

### Notification Navigation
The service includes navigation logic based on message type:
- `booking_confirmation` → Booking details screen
- `order_update` → Order status screen  
- `delivery_update` → Delivery tracking screen
- Default → Notifications screen

## 🛠️ Configuration Details

### Android Manifest Permissions:
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
```

### FCM Service Configuration:
```xml
<service
    android:name="io.flutter.plugins.firebase.messaging.FlutterFirebaseMessagingService"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>
```

### Default Notification Settings:
- **Icon**: Custom notification icon (`@drawable/ic_notification`)
- **Color**: App primary color (`#4ECDC4`)
- **Channel**: `teja_booking_notifications`
- **Sound**: Enabled
- **Vibration**: Enabled

## 🔧 Troubleshooting

### Common Issues:

1. **No FCM Token Generated**
   - Check Firebase project configuration
   - Verify `google-services.json` is in `android/app/`
   - Ensure internet connection

2. **Notifications Not Appearing**
   - Check notification permissions in device settings
   - Verify topic subscription
   - Check Firebase Console message status

3. **App Not Opening from Notification**
   - Verify intent filters in AndroidManifest.xml
   - Check message data payload format

### Debug Information:
The FCM service logs detailed information:
- 🔥 Service initialization
- 🔑 Token generation/refresh
- 📱 Permission status
- 📢 Topic subscription/unsubscription
- 📱/🔄/🚀 Message handling (foreground/background/terminated)

## 📊 Testing Scenarios

### Test Cases to Verify:

1. **App States**:
   - [ ] Foreground notification display
   - [ ] Background notification handling
   - [ ] Terminated state app launch

2. **Permissions**:
   - [ ] Initial permission request
   - [ ] Permission denial handling
   - [ ] Permission re-request

3. **Topic Management**:
   - [ ] Automatic subscription on app start
   - [ ] Manual subscribe/unsubscribe
   - [ ] Topic message delivery

4. **Token Management**:
   - [ ] Initial token generation
   - [ ] Token refresh on app restart
   - [ ] Token persistence

## 🚀 Next Steps

### Integration with Booking System:
To integrate with your booking system, modify the navigation logic in `FCMService._navigateBasedOnData()` to:

1. Navigate to specific screens based on message type
2. Pass booking data to destination screens
3. Update app state based on notification data

### Custom Message Types:
Add support for additional message types:
- `order_cancelled`
- `delivery_delayed` 
- `payment_reminder`
- `promotional_offer`

### Advanced Features:
- **Image Notifications**: Add image support for promotional messages
- **Action Buttons**: Add quick action buttons to notifications
- **Notification Grouping**: Group related notifications
- **Notification History**: Store notification history in local database

## 📝 Notes

- FCM tokens are automatically refreshed and saved to SharedPreferences
- The service is initialized in `main.dart` before app startup
- Background message handler is registered as a top-level function
- All debug information is logged with emoji prefixes for easy identification
- The implementation follows Firebase best practices for Flutter integration 