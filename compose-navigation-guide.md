# 🧭 Jetpack Compose Activity Navigation - Study Guide

## 📖 What is Activity Navigation?

**Activity Navigation** means moving from one screen (Activity) to another in your Android app. You can also pass data between activities.

### 🔑 Key Facts:
- Use `Intent` to navigate between activities
- Use `putExtra()` to send data
- Use `getStringExtra()`, `getIntExtra()`, etc. to receive data
- Always declare activities in `AndroidManifest.xml`

---

## 🚀 Basic Navigation - No Data

### First Activity (MainActivity)

```kotlin
package com.example.navigation

import android.content.Intent
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                MainScreen()
            }
        }
    }
}

@Composable
fun MainScreen() {
    val context = LocalContext.current
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(
            "First Screen",
            style = MaterialTheme.typography.headlineMedium
        )
        
        Spacer(Modifier.height(16.dp))
        
        Button(onClick = {
            val intent = Intent(context, SecondActivity::class.java)
            context.startActivity(intent)
        }) {
            Text("Go to Second Screen")
        }
    }
}
```

### Second Activity (SecondActivity)

```kotlin
package com.example.navigation

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class SecondActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                SecondScreen()
            }
        }
    }
}

@Composable
fun SecondScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(
            "Second Screen",
            style = MaterialTheme.typography.headlineMedium
        )
        
        Text("You navigated successfully!")
    }
}
```

---

## 📦 Passing Data - Simple Types

### Sending String Data

```kotlin
package com.example.navigation

import android.content.Intent
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.unit.dp

class SendDataActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                SendDataScreen()
            }
        }
    }
}

@Composable
fun SendDataScreen() {
    val context = LocalContext.current
    var name by remember { mutableStateOf("") }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Enter Your Name", style = MaterialTheme.typography.headlineMedium)
        
        Spacer(Modifier.height(16.dp))
        
        OutlinedTextField(
            value = name,
            onValueChange = { name = it },
            label = { Text("Name") }
        )
        
        Spacer(Modifier.height(16.dp))
        
        Button(onClick = {
            val intent = Intent(context, ReceiveDataActivity::class.java)
            intent.putExtra("USER_NAME", name)
            context.startActivity(intent)
        }) {
            Text("Send Name")
        }
    }
}
```

### Receiving String Data

```kotlin
package com.example.navigation

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class ReceiveDataActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        val userName = intent.getStringExtra("USER_NAME") ?: "Guest"
        
        setContent {
            MaterialTheme {
                ReceiveDataScreen(userName)
            }
        }
    }
}

@Composable
fun ReceiveDataScreen(userName: String) {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Welcome!", style = MaterialTheme.typography.headlineMedium)
        
        Spacer(Modifier.height(16.dp))
        
        Text(
            "Hello, $userName!",
            style = MaterialTheme.typography.titleLarge
        )
    }
}
```

---

## 📊 Passing Multiple Data Types

### Sending Multiple Data

```kotlin
package com.example.navigation

import android.content.Intent
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.unit.dp

class SendMultipleDataActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                SendMultipleDataScreen()
            }
        }
    }
}

@Composable
fun SendMultipleDataScreen() {
    val context = LocalContext.current
    var name by remember { mutableStateOf("") }
    var age by remember { mutableStateOf("") }
    var isStudent by remember { mutableStateOf(false) }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text("User Profile", style = MaterialTheme.typography.headlineMedium)
        
        OutlinedTextField(
            value = name,
            onValueChange = { name = it },
            label = { Text("Name") },
            modifier = Modifier.fillMaxWidth()
        )
        
        OutlinedTextField(
            value = age,
            onValueChange = { age = it },
            label = { Text("Age") },
            modifier = Modifier.fillMaxWidth()
        )
        
        Row(verticalAlignment = Alignment.CenterVertically) {
            Checkbox(
                checked = isStudent,
                onCheckedChange = { isStudent = it }
            )
            Text("Student")
        }
        
        Button(
            onClick = {
                val intent = Intent(context, ReceiveMultipleDataActivity::class.java)
                intent.putExtra("USER_NAME", name)
                intent.putExtra("USER_AGE", age.toIntOrNull() ?: 0)
                intent.putExtra("IS_STUDENT", isStudent)
                context.startActivity(intent)
            },
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("Submit Profile")
        }
    }
}
```

### Receiving Multiple Data

```kotlin
package com.example.navigation

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class ReceiveMultipleDataActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        val userName = intent.getStringExtra("USER_NAME") ?: "Unknown"
        val userAge = intent.getIntExtra("USER_AGE", 0)
        val isStudent = intent.getBooleanExtra("IS_STUDENT", false)
        
        setContent {
            MaterialTheme {
                ReceiveMultipleDataScreen(userName, userAge, isStudent)
            }
        }
    }
}

@Composable
fun ReceiveMultipleDataScreen(name: String, age: Int, isStudent: Boolean) {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Profile Summary", style = MaterialTheme.typography.headlineMedium)
        
        Spacer(Modifier.height(24.dp))
        
        Card(
            modifier = Modifier.fillMaxWidth(),
            elevation = CardDefaults.cardElevation(4.dp)
        ) {
            Column(modifier = Modifier.padding(16.dp)) {
                Text("Name: $name")
                Text("Age: $age")
                Text("Student: ${if (isStudent) "Yes" else "No"}")
            }
        }
    }
}
```

---

## 🔄 Navigation with Back Result

### Activity Requesting Result

```kotlin
package com.example.navigation

import android.content.Intent
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.result.contract.ActivityResultContracts
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.unit.dp

class RequestResultActivity : ComponentActivity() {
    private var resultText by mutableStateOf("No result yet")
    
    private val launcher = registerForActivityResult(
        ActivityResultContracts.StartActivityForResult()
    ) { result ->
        if (result.resultCode == RESULT_OK) {
            resultText = result.data?.getStringExtra("RESULT_DATA") ?: "No data"
        }
    }
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                RequestResultScreen(
                    resultText = resultText,
                    onNavigate = {
                        val intent = Intent(this, ReturnResultActivity::class.java)
                        launcher.launch(intent)
                    }
                )
            }
        }
    }
}

@Composable
fun RequestResultScreen(resultText: String, onNavigate: () -> Unit) {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("First Screen", style = MaterialTheme.typography.headlineMedium)
        
        Spacer(Modifier.height(16.dp))
        
        Text("Result: $resultText")
        
        Spacer(Modifier.height(16.dp))
        
        Button(onClick = onNavigate) {
            Text("Pick an Option")
        }
    }
}
```

### Activity Returning Result

```kotlin
package com.example.navigation

import android.content.Intent
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class ReturnResultActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                ReturnResultScreen(
                    onOptionSelected = { option ->
                        val resultIntent = Intent()
                        resultIntent.putExtra("RESULT_DATA", option)
                        setResult(RESULT_OK, resultIntent)
                        finish()
                    }
                )
            }
        }
    }
}

@Composable
fun ReturnResultScreen(onOptionSelected: (String) -> Unit) {
    val options = listOf("Option A", "Option B", "Option C")
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text("Select an Option", style = MaterialTheme.typography.headlineMedium)
        
        options.forEach { option ->
            Button(
                onClick = { onOptionSelected(option) },
                modifier = Modifier.fillMaxWidth()
            ) {
                Text(option)
            }
        }
    }
}
```

---

## 📱 AndroidManifest.xml Configuration

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/Theme.MyApp">
        
        <!-- Main Activity (Launcher) -->
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        
        <!-- Other Activities -->
        <activity android:name=".SecondActivity" />
        <activity android:name=".SendDataActivity" />
        <activity android:name=".ReceiveDataActivity" />
        <activity android:name=".SendMultipleDataActivity" />
        <activity android:name=".ReceiveMultipleDataActivity" />
        <activity android:name=".RequestResultActivity" />
        <activity android:name=".ReturnResultActivity" />
        
    </application>
    
</manifest>
```

---

## ❓ Frequently Asked Questions

### Q: What is an Intent?
**A:** An Intent is a message that tells Android to start another Activity. Think of it as a request to open another screen.

### Q: What does `LocalContext.current` do?
**A:** It gives you access to the Android Context in a Composable function, which you need to start activities.

### Q: What data types can I pass?
**A:** Common types:
- String: `putExtra("KEY", "value")` / `getStringExtra("KEY")`
- Int: `putExtra("KEY", 123)` / `getIntExtra("KEY", 0)`
- Boolean: `putExtra("KEY", true)` / `getBooleanExtra("KEY", false)`
- Double: `putExtra("KEY", 3.14)` / `getDoubleExtra("KEY", 0.0)`
- Long: `putExtra("KEY", 1000L)` / `getLongExtra("KEY", 0L)`

### Q: What if I forget to declare an Activity in AndroidManifest.xml?
**A:** Your app will crash with `ActivityNotFoundException` when trying to navigate to that activity.

### Q: What's the difference between `startActivity()` and `startActivityForResult()`?
**A:** 
- `startActivity()`: Just open another screen (one-way)
- `startActivityForResult()` (deprecated): Open screen and get data back
- **Modern way**: Use `ActivityResultContracts` (shown in the back result example)

### Q: How do I close the current activity?
**A:** Call `finish()` or add a back button:
```kotlin
Button(onClick = { (context as? ComponentActivity)?.finish() }) {
    Text("Go Back")
}
```

### Q: Can I pass custom objects?
**A:** Yes, but they must implement `Parcelable` or `Serializable`. For simple apps, it's easier to pass individual fields.

---

## 🔧 Quick Reference

| Task | Code |
|------|------|
| Get context | `val context = LocalContext.current` |
| Create intent | `Intent(context, TargetActivity::class.java)` |
| Add String data | `intent.putExtra("KEY", "value")` |
| Add Int data | `intent.putExtra("KEY", 123)` |
| Start activity | `context.startActivity(intent)` |
| Get String data | `intent.getStringExtra("KEY") ?: "default"` |
| Get Int data | `intent.getIntExtra("KEY", 0)` |
| Close activity | `finish()` |

---

## 💡 Best Practices

1. **Use Constant Keys**: Define keys as constants to avoid typos
```kotlin
companion object {
    const val KEY_USER_NAME = "USER_NAME"
    const val KEY_USER_AGE = "USER_AGE"
}
```

2. **Provide Default Values**: Always provide defaults when getting data
```kotlin
val name = intent.getStringExtra("USER_NAME") ?: "Guest"
val age = intent.getIntExtra("USER_AGE", 0)
```

3. **Validate Data**: Check if data exists before using it
```kotlin
val name = intent.getStringExtra("USER_NAME")
if (name != null) {
    // Use name
}
```

4. **Use Proper Types**: Use the correct `getXxxExtra()` method for your data type

---

## 📦 Required Dependencies

```kotlin
dependencies {
    implementation "androidx.compose.ui:ui:1.5.4"
    implementation "androidx.compose.material3:material3:1.1.2"
    implementation "androidx.activity:activity-compose:1.8.1"
}
```

---

## 📚 Next Steps
For modern Android development, consider learning **Jetpack Navigation Component** which handles navigation within a single Activity using Composables instead of multiple Activities!