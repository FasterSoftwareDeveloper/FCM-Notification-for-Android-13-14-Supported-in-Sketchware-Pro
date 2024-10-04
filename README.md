# FCM Notification for Android 13 & 14 Supported in Sketchware Pro

![FCM Notification](https://i.ibb.co.com/hRzqZCB/Yellow-and-Black-Modern-Attractive-You-Tube-Thumbnail.png)  <!-- Replace with the actual path of your image -->

## Overview

**FCM Notification** is a comprehensive implementation designed for Android applications built using Sketchware Pro. This service supports Firebase Cloud Messaging (FCM) notifications, allowing admin users to send notifications via device tokens or topics. The service is optimized for Android SDK 21 and above, ensuring seamless operation across modern devices.

## Features

- **Push Notification Handling**: Efficiently receives and processes push notifications from Firebase.
- **Admin Functionality**: Admins can send notifications using:
  - Title
  - Message
  - Image URL
  - Topic or Token
  - Channel creation
  - Extra data
  - **Open another activity** upon notification click
- **Image Support**: Downloads and displays images within notifications, allowing for rich multimedia experiences.
- **Notification Click Action**: Directs users to another activity when they click on the notification.
- **Compatibility**: Full support for Android SDK 21 and above, utilizing the latest notification features.
- **User-Friendly Validation**: Demonstrates the capabilities of `TextInputLayout` for error handling and messaging.

## Installation

1. **Set Up Firebase**:
   - Add your Android app in the [Firebase Console](https://console.firebase.google.com/).
   - Download the `google-services.json` file and place it in the `app/` directory.

2. **Add Dependencies**:
   Add the following dependencies to your `build.gradle` file:

   ```groovy
   implementation 'com.google.code.gson:gson:2.11.0' // Gson library for JSON handling
   ```

3. **Modify Your AndroidManifest.xml**:
   Ensure you declare the necessary permissions and service:

   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
   <uses-permission android:name="android.permission.POST_NOTIFICATIONS"/> <!-- Required for Android 13+ -->
   <uses-permission android:name="android.permission.WAKE_LOCK"/>
   <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE"/>

   <application>
       ...
       <service
           android:name=".FCMService"
           android:directBootAware="true"
           android:exported="true">
           <intent-filter>
               <action android:name="com.google.firebase.MESSAGING_EVENT" />
           </intent-filter>
       </service>
   </application>
   ```

## Usage

1. **Sending Notifications**: Admins can send notifications through tokens or topics, specifying the following parameters:

   - **Title**: The title of the notification.
   - **Message**: The body text of the notification.
   - **Image**: URL of the image to be displayed in the notification.
   - **Topic or Token**: The target topic or device token for the notification.
   - **Channel Creation**: Create a notification channel for Android Oreo and above.
   - **Extra Data**: Any additional data you wish to include.
   - **Open Another Activity**: Configure the notification to open a specified activity when clicked.

   Here's an example of the JSON payload you might send from your server:

   ```json
   {
       "to": "DEVICE_TOKEN_OR_TOPIC",
       "data": {
           "title": "Notification Title",
           "body": "This is the body of the notification.",
           "image": "https://example.com/image.jpg",
           "extraData": "Additional data here",
           "channel": "your_channel_id"
       }
   }
   ```

2. **Notification Click Action**:
   - When a notification is clicked, it can open a specified activity. Ensure that you handle the intent in the `FCMService` to redirect users accordingly. 
   - Here’s a sample implementation of how to handle notification clicks in the `FCMService`:

   ```java
   @Override
   public void onMessageReceived(RemoteMessage remoteMessage) {
       // Extract data
       String title = remoteMessage.getData().get("title");
       String body = remoteMessage.getData().get("body");
       String imageUrl = remoteMessage.getData().get("image");
       String extraData = remoteMessage.getData().get("extraData");
       
       // Show notification
       showNotification(title, body, imageUrl, extraData);
   }

   private void showNotification(String title, String body, String imageUrl, String extraData) {
       Intent intent = new Intent(this, TargetActivity.class); // Change to your target activity
       intent.putExtra("extraData", extraData); // Pass any required data
       PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT | PendingIntent.FLAG_IMMUTABLE);

       NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
           .setSmallIcon(R.drawable.app_icon)
           .setContentTitle(title)
           .setContentText(body)
           .setContentIntent(pendingIntent)
           .setAutoCancel(true);

       // Show the notification
       NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
       notificationManager.notify((int) System.currentTimeMillis(), builder.build());
   }
   ```

3. **Authentication**:
   - **Reminder**: Replace the `service-account.json` file in the `assets` folder with your own service account file for proper authentication.

4. **Token Management**:
   - **Reminder**: Before sending a notification, ensure to generate an access token. Note that the access token is valid for 1 hour, so refresh the token as needed to maintain access.

## Validation Note

- **Note**: Validation is not a required feature for the application. The validation logic provided here is solely for demonstrating the capabilities of `TextInputLayout`, such as error handling and displaying messages. You may remove or modify these validations based on your application's requirements.

## Permissions

Ensure you have the following permissions declared in your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/> <!-- Required for Android 13+ -->
<uses-permission android:name="android.permission.WAKE_LOCK"/>
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE"/>
```

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue for any enhancements or bug fixes.

## Author

- **Bokku** - Original Author
- **Faster Software Developer** - Modified and Updated

---
