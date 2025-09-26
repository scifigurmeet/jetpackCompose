# 🎛️ Input Controls - Short & Sweet Guide

## 🎯 Quick Overview
**Input Controls** = Components that let users interact with your app

| Control | Purpose | State Type |
|---------|---------|------------|
| `TextField` | Text input | `String` |
| `Checkbox` | On/Off choice | `Boolean` |
| `RadioButton` | Single choice from group | `String/Int` |
| `DropdownMenu` | Pick from list | `String/Object` |
| `Slider` | Range selection | `Float` |

---

## 📝 TextField - Text Input

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Email
import androidx.compose.material.icons.filled.Lock
import androidx.compose.material.icons.filled.Visibility
import androidx.compose.material.icons.filled.VisibilityOff
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.text.input.VisualTransformation
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class TextFieldActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(modifier = Modifier.fillMaxSize()) {
                    TextFieldExamples()
                }
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun TextFieldExamples() {
    var name by remember { mutableStateOf("") }
    var email by remember { mutableStateOf("") }
    var password by remember { mutableStateOf("") }
    var phone by remember { mutableStateOf("") }
    var passwordVisible by remember { mutableStateOf(false) }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(20.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text("TextField Examples", fontWeight = FontWeight.Bold)
        
        // Basic TextField
        OutlinedTextField(
            value = name,
            onValueChange = { name = it },
            label = { Text("Full Name") },
            modifier = Modifier.fillMaxWidth()
        )
        
        // Email TextField
        OutlinedTextField(
            value = email,
            onValueChange = { email = it },
            label = { Text("Email") },
            leadingIcon = { Icon(Icons.Default.Email, null) },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
            modifier = Modifier.fillMaxWidth()
        )
        
        // Password TextField
        OutlinedTextField(
            value = password,
            onValueChange = { password = it },
            label = { Text("Password") },
            leadingIcon = { Icon(Icons.Default.Lock, null) },
            trailingIcon = {
                IconButton(onClick = { passwordVisible = !passwordVisible }) {
                    Icon(
                        if (passwordVisible) Icons.Default.Visibility else Icons.Default.VisibilityOff,
                        contentDescription = "Toggle password visibility"
                    )
                }
            },
            visualTransformation = if (passwordVisible) VisualTransformation.None else PasswordVisualTransformation(),
            modifier = Modifier.fillMaxWidth()
        )
        
        // Phone Number TextField
        OutlinedTextField(
            value = phone,
            onValueChange = { if (it.all { char -> char.isDigit() }) phone = it },
            label = { Text("Phone Number") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Phone),
            supportingText = { Text("Numbers only") },
            modifier = Modifier.fillMaxWidth()
        )
    }
}

@Preview
@Composable
fun TextFieldPreview() {
    MaterialTheme { TextFieldExamples() }
}
```

---

## ☑️ Checkbox - Boolean Choices

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
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class CheckboxActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(modifier = Modifier.fillMaxSize()) {
                    CheckboxExamples()
                }
            }
        }
    }
}

@Composable
fun CheckboxExamples() {
    var agreeToTerms by remember { mutableStateOf(false) }
    var newsletter by remember { mutableStateOf(true) }
    var notifications by remember { mutableStateOf(false) }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(20.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text("Checkbox Examples", fontWeight = FontWeight.Bold)
        
        // Basic Checkbox with Text
        Row(
            verticalAlignment = Alignment.CenterVertically
        ) {
            Checkbox(
                checked = agreeToTerms,
                onCheckedChange = { agreeToTerms = it }
            )
            Text("I agree to the terms and conditions")
        }
        
        // Checkbox with Card
        Card {
            Row(
                modifier = Modifier.padding(16.dp),
                verticalAlignment = Alignment.CenterVertically
            ) {
                Checkbox(
                    checked = newsletter,
                    onCheckedChange = { newsletter = it }
                )
                Spacer(Modifier.width(8.dp))
                Column {
                    Text("Subscribe to Newsletter", fontWeight = FontWeight.Medium)
                    Text("Get weekly updates", style = MaterialTheme.typography.bodySmall)
                }
            }
        }
        
        // Custom styled checkbox
        Row(
            verticalAlignment = Alignment.CenterVertically
        ) {
            Checkbox(
                checked = notifications,
                onCheckedChange = { notifications = it },
                colors = CheckboxDefaults.colors(
                    checkedColor = MaterialTheme.colorScheme.secondary
                )
            )
            Text("Enable push notifications")
        }
        
        // Status display
        Card(
            colors = CardDefaults.cardColors(containerColor = MaterialTheme.colorScheme.primaryContainer)
        ) {
            Column(modifier = Modifier.padding(16.dp)) {
                Text("Selected Options:", fontWeight = FontWeight.Bold)
                Text("Terms: ${if (agreeToTerms) "✅" else "❌"}")
                Text("Newsletter: ${if (newsletter) "✅" else "❌"}")
                Text("Notifications: ${if (notifications) "✅" else "❌"}")
            }
        }
    }
}

@Preview
@Composable
fun CheckboxPreview() {
    MaterialTheme { CheckboxExamples() }
}
```

---

## 🔘 RadioButton - Single Choice

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.selection.selectable
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class RadioButtonActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(modifier = Modifier.fillMaxSize()) {
                    RadioButtonExamples()
                }
            }
        }
    }
}

@Composable
fun RadioButtonExamples() {
    val themes = listOf("Light", "Dark", "Auto")
    var selectedTheme by remember { mutableStateOf("Auto") }
    
    val sizes = listOf("Small", "Medium", "Large")
    var selectedSize by remember { mutableStateOf("Medium") }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(20.dp),
        verticalArrangement = Arrangement.spacedBy(20.dp)
    ) {
        Text("RadioButton Examples", fontWeight = FontWeight.Bold)
        
        // Theme Selection
        Card {
            Column(modifier = Modifier.padding(16.dp)) {
                Text("Theme", fontWeight = FontWeight.Medium)
                Spacer(Modifier.height(8.dp))
                
                themes.forEach { theme ->
                    Row(
                        modifier = Modifier
                            .fillMaxWidth()
                            .selectable(
                                selected = selectedTheme == theme,
                                onClick = { selectedTheme = theme }
                            ),
                        verticalAlignment = Alignment.CenterVertically
                    ) {
                        RadioButton(
                            selected = selectedTheme == theme,
                            onClick = { selectedTheme = theme }
                        )
                        Text(theme)
                    }
                }
            }
        }
        
        // Size Selection
        Card {
            Column(modifier = Modifier.padding(16.dp)) {
                Text("Font Size", fontWeight = FontWeight.Medium)
                Spacer(Modifier.height(8.dp))
                
                sizes.forEach { size ->
                    Row(
                        modifier = Modifier
                            .fillMaxWidth()
                            .selectable(
                                selected = selectedSize == size,
                                onClick = { selectedSize = size }
                            ),
                        verticalAlignment = Alignment.CenterVertically
                    ) {
                        RadioButton(
                            selected = selectedSize == size,
                            onClick = { selectedSize = size }
                        )
                        Text("$size text")
                    }
                }
            }
        }
        
        // Preview
        Card(
            colors = CardDefaults.cardColors(containerColor = MaterialTheme.colorScheme.secondaryContainer)
        ) {
            Text(
                "Preview: $selectedTheme theme with $selectedSize text",
                modifier = Modifier.padding(16.dp),
                fontWeight = FontWeight.Bold
            )
        }
    }
}

@Preview
@Composable
fun RadioButtonPreview() {
    MaterialTheme { RadioButtonExamples() }
}
```

---

## 📋 DropdownMenu - Selection Lists

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ArrowDropDown
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class DropdownActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(modifier = Modifier.fillMaxSize()) {
                    DropdownExamples()
                }
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun DropdownExamples() {
    val countries = listOf("USA", "Canada", "UK", "Australia", "Germany")
    var selectedCountry by remember { mutableStateOf("") }
    var countryExpanded by remember { mutableStateOf(false) }
    
    val languages = listOf("English", "Spanish", "French", "German", "Chinese")
    var selectedLanguage by remember { mutableStateOf("English") }
    var languageExpanded by remember { mutableStateOf(false) }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(20.dp),
        verticalArrangement = Arrangement.spacedBy(20.dp)
    ) {
        Text("Dropdown Examples", fontWeight = FontWeight.Bold)
        
        // Country Dropdown
        Column {
            Text("Select Country", fontWeight = FontWeight.Medium)
            Spacer(Modifier.height(4.dp))
            
            ExposedDropdownMenuBox(
                expanded = countryExpanded,
                onExpandedChange = { countryExpanded = it }
            ) {
                OutlinedTextField(
                    value = selectedCountry,
                    onValueChange = {},
                    readOnly = true,
                    placeholder = { Text("Choose country") },
                    trailingIcon = {
                        ExposedDropdownMenuDefaults.TrailingIcon(expanded = countryExpanded)
                    },
                    modifier = Modifier
                        .fillMaxWidth()
                        .menuAnchor()
                )
                
                ExposedDropdownMenu(
                    expanded = countryExpanded,
                    onDismissRequest = { countryExpanded = false }
                ) {
                    countries.forEach { country ->
                        DropdownMenuItem(
                            text = { Text(country) },
                            onClick = {
                                selectedCountry = country
                                countryExpanded = false
                            }
                        )
                    }
                }
            }
        }
        
        // Language Dropdown
        Column {
            Text("Language", fontWeight = FontWeight.Medium)
            Spacer(Modifier.height(4.dp))
            
            ExposedDropdownMenuBox(
                expanded = languageExpanded,
                onExpandedChange = { languageExpanded = it }
            ) {
                OutlinedTextField(
                    value = selectedLanguage,
                    onValueChange = {},
                    readOnly = true,
                    trailingIcon = {
                        ExposedDropdownMenuDefaults.TrailingIcon(expanded = languageExpanded)
                    },
                    modifier = Modifier
                        .fillMaxWidth()
                        .menuAnchor()
                )
                
                ExposedDropdownMenu(
                    expanded = languageExpanded,
                    onDismissRequest = { languageExpanded = false }
                ) {
                    languages.forEach { language ->
                        DropdownMenuItem(
                            text = { Text(language) },
                            onClick = {
                                selectedLanguage = language
                                languageExpanded = false
                            }
                        )
                    }
                }
            }
        }
        
        // Results
        if (selectedCountry.isNotEmpty()) {
            Card {
                Text(
                    "Selection: $selectedCountry, $selectedLanguage",
                    modifier = Modifier.padding(16.dp),
                    fontWeight = FontWeight.Medium
                )
            }
        }
    }
}

@Preview
@Composable
fun DropdownPreview() {
    MaterialTheme { DropdownExamples() }
}
```

---

## 🎚️ Slider - Range Selection

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
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import kotlin.math.roundToInt

class SliderActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(modifier = Modifier.fillMaxSize()) {
                    SliderExamples()
                }
            }
        }
    }
}

@Composable
fun SliderExamples() {
    var volume by remember { mutableStateOf(50f) }
    var brightness by remember { mutableStateOf(75f) }
    var ageRange by remember { mutableStateOf(18f..65f) }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(20.dp),
        verticalArrangement = Arrangement.spacedBy(24.dp)
    ) {
        Text("Slider Examples", fontWeight = FontWeight.Bold)
        
        // Volume Slider
        Card {
            Column(modifier = Modifier.padding(16.dp)) {
                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween,
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Text("Volume")
                    Text("${volume.roundToInt()}%", fontWeight = FontWeight.Bold)
                }
                
                Slider(
                    value = volume,
                    onValueChange = { volume = it },
                    valueRange = 0f..100f,
                    modifier = Modifier.fillMaxWidth()
                )
            }
        }
        
        // Brightness Slider with Steps
        Card {
            Column(modifier = Modifier.padding(16.dp)) {
                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween,
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Text("Brightness")
                    Text("${brightness.roundToInt()}%", fontWeight = FontWeight.Bold)
                }
                
                Slider(
                    value = brightness,
                    onValueChange = { brightness = it },
                    valueRange = 0f..100f,
                    steps = 9, // Creates 10 positions (0, 10, 20, ..., 100)
                    modifier = Modifier.fillMaxWidth()
                )
                
                Text(
                    "Tap and hold to see step markers",
                    style = MaterialTheme.typography.bodySmall,
                    color = MaterialTheme.colorScheme.onSurfaceVariant
                )
            }
        }
        
        // Range Slider
        Card {
            Column(modifier = Modifier.padding(16.dp)) {
                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween,
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Text("Age Range")
                    Text("${ageRange.start.roundToInt()} - ${ageRange.endInclusive.roundToInt()}", fontWeight = FontWeight.Bold)
                }
                
                RangeSlider(
                    value = ageRange,
                    onValueChange = { ageRange = it },
                    valueRange = 18f..100f,
                    modifier = Modifier.fillMaxWidth()
                )
            }
        }
        
        // Preview Card
        Card(
            colors = CardDefaults.cardColors(containerColor = MaterialTheme.colorScheme.primaryContainer)
        ) {
            Column(modifier = Modifier.padding(16.dp)) {
                Text("Current Settings:", fontWeight = FontWeight.Bold)
                Text("🔊 Volume: ${volume.roundToInt()}%")
                Text("☀️ Brightness: ${brightness.roundToInt()}%")
                Text("👥 Age: ${ageRange.start.roundToInt()}-${ageRange.endInclusive.roundToInt()} years")
            }
        }
    }
}

@Preview
@Composable
fun SliderPreview() {
    MaterialTheme { SliderExamples() }
}
```

---

## 🎯 All-in-One Form Example

```kotlin
package com.example.inputcontrols

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.rememberScrollState
import androidx.compose.foundation.selection.selectable
import androidx.compose.foundation.verticalScroll
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import kotlin.math.roundToInt

class CompleteFormActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(modifier = Modifier.fillMaxSize()) {
                    CompleteForm()
                }
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun CompleteForm() {
    // All form states
    var name by remember { mutableStateOf("") }
    var email by remember { mutableStateOf("") }
    var country by remember { mutableStateOf("") }
    var countryExpanded by remember { mutableStateOf(false) }
    var gender by remember { mutableStateOf("") }
    var age by remember { mutableStateOf(25f) }
    var newsletter by remember { mutableStateOf(false) }
    var terms by remember { mutableStateOf(false) }
    
    val countries = listOf("USA", "Canada", "UK", "Australia")
    val genders = listOf("Male", "Female", "Other", "Prefer not to say")
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .verticalScroll(rememberScrollState())
            .padding(20.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text("Registration Form", fontWeight = FontWeight.Bold)
        
        // Text Fields
        OutlinedTextField(
            value = name,
            onValueChange = { name = it },
            label = { Text("Full Name") },
            modifier = Modifier.fillMaxWidth()
        )
        
        OutlinedTextField(
            value = email,
            onValueChange = { email = it },
            label = { Text("Email") },
            modifier = Modifier.fillMaxWidth()
        )
        
        // Dropdown
        ExposedDropdownMenuBox(
            expanded = countryExpanded,
            onExpandedChange = { countryExpanded = it }
        ) {
            OutlinedTextField(
                value = country,
                onValueChange = {},
                readOnly = true,
                label = { Text("Country") },
                trailingIcon = { ExposedDropdownMenuDefaults.TrailingIcon(expanded = countryExpanded) },
                modifier = Modifier.fillMaxWidth().menuAnchor()
            )
            
            ExposedDropdownMenu(
                expanded = countryExpanded,
                onDismissRequest = { countryExpanded = false }
            ) {
                countries.forEach { item ->
                    DropdownMenuItem(
                        text = { Text(item) },
                        onClick = { country = item; countryExpanded = false }
                    )
                }
            }
        }
        
        // Radio Buttons
        Card {
            Column(modifier = Modifier.padding(16.dp)) {
                Text("Gender", fontWeight = FontWeight.Medium)
                genders.forEach { option ->
                    Row(
                        modifier = Modifier.selectable(
                            selected = gender == option,
                            onClick = { gender = option }
                        ),
                        verticalAlignment = Alignment.CenterVertically
                    ) {
                        RadioButton(
                            selected = gender == option,
                            onClick = { gender = option }
                        )
                        Text(option)
                    }
                }
            }
        }
        
        // Slider
        Card {
            Column(modifier = Modifier.padding(16.dp)) {
                Text("Age: ${age.roundToInt()}", fontWeight = FontWeight.Medium)
                Slider(
                    value = age,
                    onValueChange = { age = it },
                    valueRange = 18f..100f
                )
            }
        }
        
        // Checkboxes
        Row(verticalAlignment = Alignment.CenterVertically) {
            Checkbox(checked = newsletter, onCheckedChange = { newsletter = it })
            Text("Subscribe to newsletter")
        }
        
        Row(verticalAlignment = Alignment.CenterVertically) {
            Checkbox(checked = terms, onCheckedChange = { terms = it })
            Text("I agree to terms and conditions")
        }
        
        // Submit Button
        Button(
            onClick = { /* Handle submit */ },
            enabled = name.isNotEmpty() && email.isNotEmpty() && terms,
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("Submit")
        }
        
        // Form Summary
        if (name.isNotEmpty()) {
            Card(colors = CardDefaults.cardColors(containerColor = MaterialTheme.colorScheme.secondaryContainer)) {
                Column(modifier = Modifier.padding(16.dp)) {
                    Text("Form Summary:", fontWeight = FontWeight.Bold)
                    Text("Name: $name")
                    Text("Email: $email")
                    Text("Country: $country")
                    Text("Gender: $gender")
                    Text("Age: ${age.roundToInt()}")
                    Text("Newsletter: ${if (newsletter) "Yes" else "No"}")
                    Text("Terms: ${if (terms) "Accepted" else "Not accepted"}")
                }
            }
        }
    }
}

@Preview
@Composable
fun CompleteFormPreview() {
    MaterialTheme { CompleteForm() }
}
```

---

## ❓ Quick FAQs

**Q: When to use TextField vs OutlinedTextField?**
A: `OutlinedTextField` is more modern and better for forms. Use `TextField` for simple cases.

**Q: How to validate form inputs?**
A: Use `isError` parameter and `supportingText` for validation messages.

**Q: RadioButton vs Checkbox?**
A: RadioButton = choose one from group. Checkbox = multiple independent choices.

**Q: How to handle Dropdown with objects?**
A: Store the object in state, display object.name in dropdown.

**Q: Can I customize Slider appearance?**
A: Yes! Use `colors` parameter in SliderDefaults.colors().

---

## 🎯 Quick Reference

```kotlin
// Essential patterns for each control:

// TextField
var text by remember { mutableStateOf("") }
OutlinedTextField(value = text, onValueChange = { text = it })

// Checkbox  
var checked by remember { mutableStateOf(false) }
Checkbox(checked = checked, onCheckedChange = { checked = it })

// RadioButton
var selected by remember { mutableStateOf("") }
RadioButton(selected = selected == "option", onClick = { selected = "option" })

// Dropdown
var expanded by remember { mutableStateOf(false) }
ExposedDropdownMenuBox(expanded = expanded, onExpandedChange = { expanded = it })

// Slider
var value by remember { mutableStateOf(50f) }
Slider(value = value, onValueChange = { value = it })
```

## 🔧 Required Dependencies
```kotlin
implementation "androidx.compose.material3:material3:1.1.2"
implementation "androidx.compose.material:material-icons-extended:1.5.4"
```

**🎯 Key Takeaway: All input controls follow the same pattern - state + callback!**
