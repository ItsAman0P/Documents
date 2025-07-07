# 📲 MSG91 Push Notification SDK for React Native

Easily integrate **in-app pop-up notifications** and **Firebase Cloud Messaging (FCM)** notifications in your React Native app using MSG91's Push Notification SDK.

Supports:
- 🔔 In-App HTML Pop-up Notifications
- 📩 FCM Notifications on Android & iOS with HTML Templates

---

## 🚀 Installation

```bash
npm install @msg91comm/MSG91PushNotificationSDK
# or
yarn add @msg91comm/MSG91PushNotificationSDK
```

---

## 📦 Requirements

- [react-native-webview](https://www.npmjs.com/package/react-native-webview) - Install this as our package requires it as peer dependency.
#### For FCM Push-Notifications Only:
- [@react-native-firebase/messaging](https://rnfirebase.io/messaging/usage) - for FCM Notifications
- Firebase project with FCM enabled

**Important:** If you want to have FCM notifications in your app.

---

## 🔧 Firebase Setup (for FCM Notifications)

To send FCM notifications, your app must be connected to a Firebase project.

### 1. Generate Firebase Admin SDK Key

- Go to your Firebase project.
- Navigate to **Project Settings > Service Accounts**.
- Click **"Generate new private key"** and download the JSON file.



| Step 1 | Step 2 | Step 3 | Step 4 |
|--------|--------|--------|--------|
| ![Step1](./assets/step1.png) | ![Step2](./assets/step2.png) | ![Step3](./assets/step3.png) | ![Step4](./assets/step4.png) |


### 2. Upload Firebase Key to MSG91 Dashboard

Upload this Firebase Admin JSON in the **Push Notification Panel** via the Firebase Integration section.

### 3. Add `google-services.json` (Android)

Place your `google-services.json` file inside:
```
android/app/google-services.json
```

### 4. Enable FCM in Your App

Follow the [official Firebase setup guide](https://rnfirebase.io/messaging/usage) to enable FCM support in your app.

---

## 💻 Usage

### Basic Setup

```tsx
import MSG91PushNotificationSDK from '@msg91comm/MSG91PushNotificationSDK';
import { SafeAreaView } from 'react-native';

<SafeAreaView>
  <MSG91PushNotificationSDK
    helloConfig={{ widgetToken: 'your_widget_token' }}
    projectId="your_firebase_project_id"
  />
</SafeAreaView>
```

### Register FCM Token

Call `registerFCM()` once you receive the FCM token from Firebase:

```ts
import messaging from '@react-native-firebase/messaging';

const fcmToken = await messaging().getToken();
MSG91PushNotificationSDK.registerFCM(fcmToken);
```

This will automatically register the token with MSG91’s backend by updating the SDK config and hitting `/add-user-fcm-token`.

### Handle FCM Notification Click

When user opens an FCM notification, you can call following on app open to show the HTML Template Notification:

```ts
  messaging().onNotificationOpenedApp((remoteMessage) => {
    if (remoteMessage?.data?.key === 'msg91') {
      MSG91PushNotificationSDK.handleFCMNotification(remoteMessage?.data?.html_url as string);
    }
  });


  messaging().onMessage((remoteMessage) => {
    if (remoteMessage?.data?.key === 'msg91') {
      MSG91PushNotificationSDK.handleFCMNotification(remoteMessage?.data?.html_url as string);
    }
  });
```

This will render the HTML content sent via FCM inside a popup.

---

## ⚙️ Props

| Prop        | Type     | Required | Description                              |
|-------------|----------|----------|------------------------------------------|
| `helloConfig` | object | Yes | Hello widget configuration, must include `widgetToken` |
| `projectId` | string   | Yes | Firebase project ID used to map user & send FCM |

---

## 🧪 SDK Methods

| Method | Description |
|--------|-------------|
| `MSG91PushNotificationSDK.registerFCM(fcmToken: string)` | Registers user device FCM token with project ID |
| `MSG91PushNotificationSDK.handleFCMNotification(htmlUrl: string)` | Opens FCM notification as Popup |

---

## 🧑‍💻 Example with Firebase Messaging

```ts
import React, { useEffect } from 'react';
import messaging from '@react-native-firebase/messaging';
import MSG91PushNotificationSDK from '@msg91comm/MSG91PushNotificationSDK';

export default function App() {
  useEffect(() => {
    messaging().getToken().then(token => {
      MSG91PushNotificationSDK.registerFCM(token);
    });

    messaging().onNotificationOpenedApp(remoteMessage => {
      if (remoteMessage?.data?.key === 'msg91') {
        MSG91PushNotificationSDK.handleFCMNotification(remoteMessage?.data?.html_url as string);
      }
    });

    messaging().onMessage((remoteMessage) => {
      if (remoteMessage?.data?.key === 'msg91') {
        MSG91PushNotificationSDK.handleFCMNotification(remoteMessage?.data?.html_url as string);
      }
    });
  }, []);

  return (
    <>
      <AppNavigator/>
      <MSG91PushNotificationSDK
        helloConfig={{ widgetToken: 'your_widget_token' }}
        projectId="your_firebase_project_id"
      />
    </>
  );
}
```