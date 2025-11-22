# Android Notification Demo with Jetpack Compose

A complete beginner's guide to creating notifications in Android using Jetpack Compose and Kotlin.

## Table of Contents
1. [Project Setup](#project-setup)
2. [Understanding the Code](#understanding-the-code)
3. [Complete Code](#complete-code)
4. [Testing the App](#testing-the-app)

---

## Project Setup

### Step 1: Create a New Project
1. Open Android Studio
2. Create a new project with "Empty Activity"
3. Select Kotlin as the language
4. Set minimum SDK to API 24 (Android 7.0) or higher

### Step 2: Add Dependencies

Open `build.gradle` (Module: app) and ensure these dependencies are included:

```gradle
dependencies {
    implementation "androidx.core:core-ktx:1.12.0"
    implementation "androidx.compose.ui:ui:1.5.4"
    implementation "androidx.compose.material3:material3:1.1.2"
    implementation "androidx.activity:activity-compose:1.8.1"
    implementation "androidx.compose.ui:ui-tooling-preview:1.5.4"
}
```

### Step 3: Add Permissions

Open `AndroidManifest.xml` and add this permission inside the `<manifest>` tag:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
    
    <application
        ...>
        <!-- Your activity here -->
    </application>
</manifest>
```

**Why?** Starting from Android 13 (API 33), apps need explicit permission to post notifications.

---

## Understanding the Code

### What are Notifications?

Notifications are messages that appear outside your app's UI, typically in the notification drawer. They can alert users about updates, messages, or important events.

### Key Concepts

#### 1. Notification Channel (Android 8.0+)
Before sending notifications, you must create a notification channel. Think of it as a category for your notifications.

```kotlin
private fun createNotificationChannel() {
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        val channel = NotificationChannel(
            CHANNEL_ID,              // Unique ID for this channel
            "Demo Notifications",    // User-visible name
            NotificationManager.IMPORTANCE_DEFAULT  // Importance level
        )
        
        val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        notificationManager.createNotificationChannel(channel)
    }
}
```

#### 2. Permission Handling (Android 13+)
Modern Android requires runtime permission to show notifications:

```kotlin
private val requestPermissionLauncher = registerForActivityResult(
    ActivityResultContracts.RequestPermission()
) { isGranted: Boolean ->
    if (isGranted) {
        showNotification()
    }
}
```

#### 3. Building a Notification
Use `NotificationCompat.Builder` to create a notification:

```kotlin
val notification = NotificationCompat.Builder(this, CHANNEL_ID)
    .setSmallIcon(android.R.drawable.ic_dialog_info)  // Required icon
    .setContentTitle("Hello!")                         // Title
    .setContentText("This is a demo notification")    // Message
    .setPriority(NotificationCompat.PRIORITY_DEFAULT) // Priority
    .build()
```

#### 4. Jetpack Compose UI
Instead of XML layouts, we use Compose functions to build UI:

```kotlin
@Composable
fun NotificationScreen(onShowNotification: () -> Unit) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(text = "Notification Demo")
        Button(onClick = onShowNotification) {
            Text("Show Notification")
        }
    }
}
```

---

## Complete Code

### MainActivity.kt

```kotlin
package com.example.notificationdemo

import android.Manifest
import android.app.NotificationChannel
import android.app.NotificationManager
import android.content.Context
import android.content.pm.PackageManager
import android.os.Build
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.result.contract.ActivityResultContracts
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.core.app.NotificationCompat
import androidx.core.app.NotificationManagerCompat
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    
    // Step 1: Register permission launcher
    // This handles the result when user allows/denies permission
    private val requestPermissionLauncher = registerForActivityResult(
        ActivityResultContracts.RequestPermission()
    ) { isGranted: Boolean ->
        if (isGranted) {
            showNotification()
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        // Step 2: Create notification channel when app starts
        createNotificationChannel()
        
        // Step 3: Set up Compose UI
        setContent {
            MaterialTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    NotificationScreen(
                        onShowNotification = { 
                            checkPermissionAndShowNotification() 
                        }
                    )
                }
            }
        }
    }

    // Step 4: Create notification channel
    // Required for Android 8.0 (API 26) and higher
    private fun createNotificationChannel() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(
                CHANNEL_ID,
                "Demo Notifications",
                NotificationManager.IMPORTANCE_DEFAULT
            ).apply {
                description = "Notification demo channel"
            }
            
            val notificationManager = 
                getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
            notificationManager.createNotificationChannel(channel)
        }
    }

    // Step 5: Check permission before showing notification
    private fun checkPermissionAndShowNotification() {
        // Android 13 (API 33) and higher require runtime permission
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
            when {
                ContextCompat.checkSelfPermission(
                    this,
                    Manifest.permission.POST_NOTIFICATIONS
                ) == PackageManager.PERMISSION_GRANTED -> {
                    // Permission already granted
                    showNotification()
                }
                else -> {
                    // Request permission
                    requestPermissionLauncher.launch(
                        Manifest.permission.POST_NOTIFICATIONS
                    )
                }
            }
        } else {
            // For older Android versions, no permission needed
            showNotification()
        }
    }

    // Step 6: Show the notification
    private fun showNotification() {
        // Build the notification
        val notification = NotificationCompat.Builder(this, CHANNEL_ID)
            .setSmallIcon(android.R.drawable.ic_dialog_info)
            .setContentTitle("Hello!")
            .setContentText("This is a demo notification")
            .setPriority(NotificationCompat.PRIORITY_DEFAULT)
            .build()

        // Display the notification
        with(NotificationManagerCompat.from(this)) {
            if (ContextCompat.checkSelfPermission(
                    this@MainActivity,
                    Manifest.permission.POST_NOTIFICATIONS
                ) == PackageManager.PERMISSION_GRANTED || 
                Build.VERSION.SDK_INT < Build.VERSION_CODES.TIRAMISU
            ) {
                notify(NOTIFICATION_ID, notification)
            }
        }
    }

    companion object {
        private const val CHANNEL_ID = "demo_channel"
        private const val NOTIFICATION_ID = 1
    }
}

// Step 7: Create the Compose UI
@Composable
fun NotificationScreen(onShowNotification: () -> Unit) {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(
            text = "Notification Demo",
            style = MaterialTheme.typography.headlineMedium
        )
        
        Spacer(modifier = Modifier.height(32.dp))
        
        Button(onClick = onShowNotification) {
            Text("Show Notification")
        }
    }
}
```

---

## Code Breakdown

### Flow of Execution

1. **App starts** → `onCreate()` is called
2. **Create channel** → `createNotificationChannel()` sets up the notification channel
3. **UI displays** → Jetpack Compose shows a button
4. **User clicks button** → `checkPermissionAndShowNotification()` is triggered
5. **Check permission** → If Android 13+, check/request POST_NOTIFICATIONS permission
6. **Show notification** → `showNotification()` creates and displays the notification

### Important Constants

```kotlin
companion object {
    private const val CHANNEL_ID = "demo_channel"  // Must match in channel & builder
    private const val NOTIFICATION_ID = 1          // Unique ID for each notification
}
```

- **CHANNEL_ID**: Identifies which channel to use
- **NOTIFICATION_ID**: Each notification needs a unique ID. Using the same ID replaces the previous notification.

---

## Testing the App

### Steps to Test

1. **Run the app** on a device or emulator (API 24+)
2. **Click the "Show Notification" button**
3. **On Android 13+**: You'll see a permission dialog - tap "Allow"
4. **Check the notification drawer**: Swipe down from the top of the screen
5. **You should see**: A notification with the title "Hello!" and message "This is a demo notification"

### Troubleshooting

**Problem**: No notification appears
- **Solution 1**: Check that you've added the permission in AndroidManifest.xml
- **Solution 2**: On Android 13+, ensure you granted permission
- **Solution 3**: Check notification settings for your app in device settings

**Problem**: App crashes
- **Solution**: Ensure all dependencies are properly added in build.gradle

**Problem**: Permission dialog doesn't appear
- **Solution**: You might have already denied permission. Go to Settings → Apps → Your App → Notifications and enable them manually

---

## Customization Ideas

### Change the Icon
```kotlin
.setSmallIcon(android.R.drawable.ic_notification_overlay)
```

### Add Action Buttons
```kotlin
.addAction(android.R.drawable.ic_menu_view, "View", pendingIntent)
```

### Change Priority
```kotlin
.setPriority(NotificationCompat.PRIORITY_HIGH)  // Makes notification pop up
```

### Add Big Text
```kotlin
.setStyle(NotificationCompat.BigTextStyle()
    .bigText("This is a much longer notification text that will expand"))
```

---

## Key Takeaways

- **Notification Channels** are required for Android 8.0+
- **Runtime Permission** is required for Android 13+
- **NotificationCompat** provides backward compatibility
- **Jetpack Compose** simplifies UI creation with declarative code
- Each notification needs a **unique channel ID** and **notification ID**

---

## Next Steps

- Learn about **PendingIntent** to make notifications clickable
- Explore **notification actions** (buttons in notifications)
- Study **notification styles** (BigPicture, BigText, Inbox)
- Implement **foreground services** with notifications

Happy coding! 🚀
