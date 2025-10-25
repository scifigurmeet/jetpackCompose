# 🎮 Jetpack Compose Input Controls - Study Guide

## 📖 What Are Input Controls?

**Input Controls** are composables that let users interact with your app by entering data, making selections, or toggling options.

### 🔑 Key Facts:
- Handle user input and selections
- Require **state management** with `remember` and `mutableStateOf`
- Use callbacks like `onValueChange` to respond to changes
- Always provide labels for accessibility

---

## 📝 Text Input

### TextField - Basic Text Input

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class TextFieldActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                TextFieldExample()
            }
        }
    }
}

@Composable
fun TextFieldExample() {
    var text by remember { mutableStateOf("") }
    
    Column(modifier = Modifier.padding(16.dp)) {
        TextField(
            value = text,
            onValueChange = { text = it },
            label = { Text("Name") }
        )
        Text("You typed: $text")
    }
}
```

### OutlinedTextField - Material Design Outlined Style

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class OutlinedTextFieldActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                OutlinedTextFieldExample()
            }
        }
    }
}

@Composable
fun OutlinedTextFieldExample() {
    var email by remember { mutableStateOf("") }
    
    Column(modifier = Modifier.padding(16.dp)) {
        OutlinedTextField(
            value = email,
            onValueChange = { email = it },
            label = { Text("Email") },
            placeholder = { Text("example@email.com") }
        )
    }
}
```

---

## ☑️ Selection Controls

### Checkbox - Toggle Selection

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class CheckboxActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                CheckboxExample()
            }
        }
    }
}

@Composable
fun CheckboxExample() {
    var checked by remember { mutableStateOf(false) }
    
    Column(modifier = Modifier.padding(16.dp)) {
        Row(verticalAlignment = Alignment.CenterVertically) {
            Checkbox(
                checked = checked,
                onCheckedChange = { checked = it }
            )
            Text("Accept terms")
        }
        Text("Accepted: $checked")
    }
}
```

### Switch - Toggle On/Off

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class SwitchActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                SwitchExample()
            }
        }
    }
}

@Composable
fun SwitchExample() {
    var enabled by remember { mutableStateOf(false) }
    
    Column(modifier = Modifier.padding(16.dp)) {
        Row(verticalAlignment = Alignment.CenterVertically) {
            Text("Notifications")
            Spacer(Modifier.width(8.dp))
            Switch(
                checked = enabled,
                onCheckedChange = { enabled = it }
            )
        }
    }
}
```

### RadioButton - Single Choice Selection

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class RadioButtonActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                RadioButtonExample()
            }
        }
    }
}

@Composable
fun RadioButtonExample() {
    var selected by remember { mutableStateOf("Option 1") }
    val options = listOf("Option 1", "Option 2", "Option 3")
    
    Column(modifier = Modifier.padding(16.dp)) {
        options.forEach { option ->
            Row(verticalAlignment = Alignment.CenterVertically) {
                RadioButton(
                    selected = selected == option,
                    onClick = { selected = option }
                )
                Text(option)
            }
        }
        Text("Selected: $selected")
    }
}
```

---

## 🎚️ Value Selection Controls

### Slider - Select from Range

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class SliderActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                SliderExample()
            }
        }
    }
}

@Composable
fun SliderExample() {
    var value by remember { mutableStateOf(50f) }
    
    Column(modifier = Modifier.padding(16.dp)) {
        Text("Volume: ${value.toInt()}")
        Slider(
            value = value,
            onValueChange = { value = it },
            valueRange = 0f..100f
        )
    }
}
```

### RangeSlider - Select Range

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class RangeSliderActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                RangeSliderExample()
            }
        }
    }
}

@Composable
fun RangeSliderExample() {
    var range by remember { mutableStateOf(20f..80f) }
    
    Column(modifier = Modifier.padding(16.dp)) {
        Text("Range: ${range.start.toInt()} - ${range.endInclusive.toInt()}")
        RangeSlider(
            value = range,
            onValueChange = { range = it },
            valueRange = 0f..100f
        )
    }
}
```

---

## 🎯 Complete Example - Login Form

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.unit.dp

class LoginFormActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                LoginForm()
            }
        }
    }
}

@Composable
fun LoginForm() {
    var username by remember { mutableStateOf("") }
    var password by remember { mutableStateOf("") }
    var rememberMe by remember { mutableStateOf(false) }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text("Login", style = MaterialTheme.typography.headlineMedium)
        
        OutlinedTextField(
            value = username,
            onValueChange = { username = it },
            label = { Text("Username") },
            modifier = Modifier.fillMaxWidth()
        )
        
        OutlinedTextField(
            value = password,
            onValueChange = { password = it },
            label = { Text("Password") },
            visualTransformation = PasswordVisualTransformation(),
            modifier = Modifier.fillMaxWidth()
        )
        
        Row {
            Checkbox(
                checked = rememberMe,
                onCheckedChange = { rememberMe = it }
            )
            Text("Remember me")
        }
        
        Button(
            onClick = { println("Login clicked") },
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("Login")
        }
    }
}
```

---

## 🎯 Complete Example - Settings Screen

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class SettingsActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                SettingsScreen()
            }
        }
    }
}

@Composable
fun SettingsScreen() {
    var notifications by remember { mutableStateOf(true) }
    var darkMode by remember { mutableStateOf(false) }
    var fontSize by remember { mutableStateOf(16f) }
    var theme by remember { mutableStateOf("Light") }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text("Settings", style = MaterialTheme.typography.headlineMedium)
        
        // Switch example
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.SpaceBetween,
            verticalAlignment = Alignment.CenterVertically
        ) {
            Text("Enable Notifications")
            Switch(
                checked = notifications,
                onCheckedChange = { notifications = it }
            )
        }
        
        // Checkbox example
        Row(verticalAlignment = Alignment.CenterVertically) {
            Checkbox(
                checked = darkMode,
                onCheckedChange = { darkMode = it }
            )
            Text("Dark Mode")
        }
        
        // Slider example
        Column {
            Text("Font Size: ${fontSize.toInt()}")
            Slider(
                value = fontSize,
                onValueChange = { fontSize = it },
                valueRange = 12f..24f
            )
        }
        
        // RadioButton example
        Text("Theme")
        listOf("Light", "Dark", "Auto").forEach { option ->
            Row(verticalAlignment = Alignment.CenterVertically) {
                RadioButton(
                    selected = theme == option,
                    onClick = { theme = option }
                )
                Text(option)
            }
        }
    }
}
```

---

## ❓ Frequently Asked Questions

### Q: What is `remember` and why do I need it?
**A:** `remember` stores state across recompositions. Without it, your input values would reset every time the UI updates.

### Q: What's the difference between TextField and OutlinedTextField?
**A:** 
- **TextField**: Filled background style
- **OutlinedTextField**: Outlined border style (more common in Material Design)

### Q: How do I make a password field?
**A:** Use `visualTransformation = PasswordVisualTransformation()` on TextField/OutlinedTextField.

### Q: When should I use Checkbox vs Switch?
**A:** 
- **Checkbox**: Multiple selections, accepting terms
- **Switch**: Toggling features on/off (like notifications)

### Q: How do I validate input?
**A:** Check the value in `onValueChange` and set an error state or disable the submit button accordingly.

---

## 🔧 Quick Reference

| Control | Purpose | Key Properties | State Type |
|---------|---------|----------------|------------|
| `TextField` | Text input | value, onValueChange, label | String |
| `OutlinedTextField` | Outlined text input | value, onValueChange, label | String |
| `Checkbox` | Multiple selections | checked, onCheckedChange | Boolean |
| `Switch` | Toggle on/off | checked, onCheckedChange | Boolean |
| `RadioButton` | Single choice | selected, onClick | String/Int |
| `Slider` | Select value from range | value, onValueChange, valueRange | Float |
| `RangeSlider` | Select range | value, onValueChange, valueRange | ClosedFloatingPointRange |

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
Once comfortable with input controls, learn about **Form Validation**, **State Management**, and **LazyColumn** for scrollable lists!
