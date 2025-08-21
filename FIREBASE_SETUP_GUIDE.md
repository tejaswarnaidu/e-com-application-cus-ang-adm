# Firebase Setup Guide for DailyMilk Agent App

## Step 1: Get Firebase Credentials

1. **Go to [Firebase Console](https://console.firebase.google.com/)**
2. **Select your "teja" project**
3. **Go to Project Settings** (gear icon)
4. **Scroll down to "Your apps" section**
5. **Click "Add app" → Android**
6. **Package name**: `com.example.dailymilk_agent`
7. **App nickname**: `DailyMilk Agent`
8. **Click "Register app"**

## Step 2: Download Configuration

1. **Download the `google-services.json` file**
2. **Place it in**: `android/app/google-services.json`
3. **Copy the configuration values** from the Firebase console

## Step 3: Update Firebase Options

Replace the placeholder values in `lib/firebase_options.dart`:

```dart
static const FirebaseOptions android = FirebaseOptions(
  apiKey: 'YOUR_ACTUAL_API_KEY_HERE', // From Firebase Console
  appId: 'YOUR_ACTUAL_APP_ID_HERE', // From Firebase Console  
  messagingSenderId: 'YOUR_ACTUAL_SENDER_ID_HERE', // From Firebase Console
  projectId: 'teja',
  storageBucket: 'teja.appspot.com',
);
```

## Step 4: Enable Authentication

1. **In Firebase Console** → **Authentication** → **Sign-in method**
2. **Enable Email/Password**
3. **Save**

## Step 5: Create Firestore Database

1. **In Firebase Console** → **Firestore Database**
2. **Click "Create database"**
3. **Choose "Start in test mode"**
4. **Select location** (choose closest to your users)
5. **Click "Done"**

## Step 6: Test the App

1. **Run**: `flutter run --debug`
2. **Try creating an account**
3. **Check Firestore** to see data in `agent_users` collection

## Collections Created:

- `agent_users` - Agent profiles
- `agent_orders` - Agent orders
- `agent_customers` - Customer data
- `agent_products` - Product data
- `agent_notifications` - Notifications
- `agent_teja_orders` - Teja app orders

All data will be stored with "agent_" prefix in your teja Firebase project! 