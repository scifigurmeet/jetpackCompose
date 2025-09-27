# Basic Jetpack Compose App

## Overview
This is a minimal Android application built with Jetpack Compose that demonstrates the fundamental UI components: Box, Column, Row, Text, and Button. The app implements a simple counter that increments when the user taps a button.

## Code

```kotlin
package com.example.basicapp

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            BasicApp()
        }
    }
}

@Composable
fun BasicApp() {
    var count by remember { mutableStateOf(0) }
    
    Box(
        modifier = Modifier.fillMaxSize(),
        contentAlignment = Alignment.Center
    ) {
        Column {
            Row {
                Text("Count: ")
                Text("$count")
            }
            Button(
                onClick = { count++ },
                modifier = Modifier.padding(top = 8.dp)
            ) {
                Text("Click")
            }
        }
    }
}
```

## Component Breakdown

### MainActivity
- **Purpose**: Entry point of the Android application
- **Key Method**: `onCreate()` sets up the Compose UI using `setContent`

### BasicApp Composable
- **State Management**: Uses `remember { mutableStateOf(0) }` to maintain counter state
- **Layout Structure**: Hierarchical arrangement of UI components

### UI Components Used

1. **Box**
   - Acts as the root container
   - Centers all content using `contentAlignment = Alignment.Center`
   - Fills the entire screen with `Modifier.fillMaxSize()`

2. **Column** 
   - Arranges child elements vertically
   - Contains the count display row and the button

3. **Row**
   - Arranges the count label and value horizontally
   - Creates "Count: X" display format

4. **Text**
   - Two instances: static label "Count: " and dynamic counter value
   - Displays textual content to the user

5. **Button**
   - Triggers counter increment on click
   - Contains "Click" text label
   - Uses `onClick = { count++ }` for state update

## App Flow

The application follows this simple interaction pattern:
1. App launches showing "Count: 0"
2. User taps the "Click" button
3. Counter increments and UI updates automatically
4. Process repeats for each button press

## Key Compose Concepts Demonstrated

- **Declarative UI**: UI is described as functions of state
- **State Management**: `remember` and `mutableStateOf` for reactive updates
- **Composition**: Nesting composables to create complex layouts
- **Modifiers**: Styling and layout behavior (`padding`, `fillMaxSize`)

## Requirements

- Android Studio with Compose support
- Minimum SDK version that supports Jetpack Compose
- Material3 design system dependencies

## UI Component Hierarchy

```mermaid
graph TD
    A[MainActivity] --> B[BasicApp Composable]
    B --> C[Box - Root Container]
    C --> D[Column - Vertical Layout]
    D --> E[Row - Horizontal Layout]
    D --> F[Button - Click Handler]
    E --> G[Text - Count: ]
    E --> H[Text - Counter Value]
    F --> I[Text - "Click"]
    
    style A fill:#e1f5fe
    style B fill:#f3e5f5
    style C fill:#fff3e0
    style D fill:#e8f5e8
    style E fill:#fff8e1
    style F fill:#ffebee
    style G fill:#f1f8e9
    style H fill:#f1f8e9
    style I fill:#f1f8e9
```
