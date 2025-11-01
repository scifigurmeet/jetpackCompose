# Currency Exchange Rate App - Documentation

## Overview
A minimal Android application using Jetpack Compose that fetches exchange rates from an API and displays the response on screen.

## API Endpoint
```
https://api.exchangerate-api.com/v4/latest/INR
```
This endpoint returns current exchange rates with INR (Indian Rupee) as the base currency.

## Prerequisites
- Android Studio (Arctic Fox or later)
- Minimum SDK: 21
- Target SDK: 34 (or latest)
- Kotlin 1.8+

## Setup Instructions

### 1. Create New Project
1. Open Android Studio
2. Create new project with "Empty Activity"
3. Select Kotlin as the language
4. Enable Jetpack Compose

### 2. Add Dependencies
Add these to your `build.gradle.kts` (Module: app):

```kotlin
dependencies {
    implementation("androidx.compose.material3:material3:1.1.2")
    implementation("androidx.activity:activity-compose:1.8.0")
}
```

### 3. Add Internet Permission
Add to `AndroidManifest.xml` (inside `<manifest>` tag):

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### 4. Main Code
Replace `MainActivity.kt` with the following:

```kotlin
package com.example.currencyapp

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import java.net.URL

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    CurrencyScreen()
                }
            }
        }
    }
}

@Composable
fun CurrencyScreen() {
    var response by remember { mutableStateOf("Loading...") }
    val scope = rememberCoroutineScope()

    LaunchedEffect(Unit) {
        scope.launch {
            response = fetchData()
        }
    }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        Text(text = response)
    }
}

suspend fun fetchData(): String {
    return withContext(Dispatchers.IO) {
        try {
            URL("https://api.exchangerate-api.com/v4/latest/INR")
                .readText()
        } catch (e: Exception) {
            "Error: ${e.message}"
        }
    }
}
```

## Code Explanation

### Components

#### MainActivity
- Sets up the Compose UI
- Entry point of the application

#### CurrencyScreen (Composable)
- Manages UI state with `remember` and `mutableStateOf`
- Uses `LaunchedEffect` to trigger API call when screen loads
- Displays the API response as text

#### fetchData (Suspend Function)
- Performs network call on IO dispatcher
- Fetches data from the API endpoint
- Returns response as String or error message

## How It Works

1. **App Launch**: MainActivity initializes the Compose UI
2. **Screen Load**: CurrencyScreen composable is displayed
3. **API Call**: LaunchedEffect triggers the fetchData() function
4. **Loading State**: "Loading..." is shown initially
5. **Data Fetch**: Network request is made on background thread
6. **Display**: Raw JSON response is displayed on screen

## Expected Response Format
```json
{
  "base": "INR",
  "date": "2025-11-01",
  "rates": {
    "USD": 0.012,
    "EUR": 0.011,
    "GBP": 0.0095,
    ...
  }
}
```

## Error Handling
- Network errors are caught and displayed as "Error: [message]"
- No internet connection will show appropriate error message

## Next Steps for Enhancement
- Parse JSON response using Gson or kotlinx.serialization
- Display rates in a formatted list
- Add pull-to-refresh functionality
- Add loading indicator
- Implement proper error UI
- Cache data locally

## Common Issues

### Network Security Exception
If you encounter a cleartext traffic error, add to `AndroidManifest.xml`:
```xml
<application
    android:usesCleartextTraffic="true"
    ...>
```

### Missing Internet Permission
Ensure the `<uses-permission>` tag is added to the manifest.

## License
Free to use for learning and development purposes.