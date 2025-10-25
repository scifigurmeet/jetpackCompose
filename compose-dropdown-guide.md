# 🔽 Jetpack Compose Dropdown - Study Guide

## 📖 What Are Dropdowns?

**Dropdowns** (also called menus or spinners) let users select one option from a list. In Jetpack Compose, dropdowns are created using `DropdownMenu` and `ExposedDropdownMenuBox`.

### 🔑 Key Facts:
- Show a list of options when clicked
- Only one option can be selected at a time
- Use `expanded` state to control visibility
- `ExposedDropdownMenuBox` is Material Design compliant

---

## 📋 Basic Dropdown Menu

### DropdownMenu - Simple Menu

```kotlin
package com.example.dropdowns

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class DropdownMenuActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                DropdownMenuExample()
            }
        }
    }
}

@Composable
fun DropdownMenuExample() {
    var expanded by remember { mutableStateOf(false) }
    var selectedOption by remember { mutableStateOf("Select Option") }
    val options = listOf("Option 1", "Option 2", "Option 3")
    
    Column(modifier = Modifier.padding(16.dp)) {
        Button(onClick = { expanded = true }) {
            Text(selectedOption)
        }
        
        DropdownMenu(
            expanded = expanded,
            onDismissRequest = { expanded = false }
        ) {
            options.forEach { option ->
                DropdownMenuItem(
                    text = { Text(option) },
                    onClick = {
                        selectedOption = option
                        expanded = false
                    }
                )
            }
        }
        
        Text("Selected: $selectedOption")
    }
}
```

---

## 🎨 ExposedDropdownMenuBox - Material Design Style

### Basic ExposedDropdownMenuBox

```kotlin
package com.example.dropdowns

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class ExposedDropdownActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                ExposedDropdownExample()
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ExposedDropdownExample() {
    var expanded by remember { mutableStateOf(false) }
    var selectedText by remember { mutableStateOf("") }
    val options = listOf("Apple", "Banana", "Cherry", "Date", "Elderberry")
    
    Column(modifier = Modifier.padding(16.dp)) {
        ExposedDropdownMenuBox(
            expanded = expanded,
            onExpandedChange = { expanded = it }
        ) {
            TextField(
                value = selectedText,
                onValueChange = {},
                readOnly = true,
                label = { Text("Select Fruit") },
                trailingIcon = { ExposedDropdownMenuDefaults.TrailingIcon(expanded = expanded) },
                modifier = Modifier.menuAnchor()
            )
            
            ExposedDropdownMenu(
                expanded = expanded,
                onDismissRequest = { expanded = false }
            ) {
                options.forEach { option ->
                    DropdownMenuItem(
                        text = { Text(option) },
                        onClick = {
                            selectedText = option
                            expanded = false
                        }
                    )
                }
            }
        }
        
        Spacer(Modifier.height(16.dp))
        Text("You selected: $selectedText")
    }
}
```

---

## 🎯 Dropdown with Icons

```kotlin
package com.example.dropdowns

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.vector.ImageVector
import androidx.compose.ui.unit.dp

class IconDropdownActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                IconDropdownExample()
            }
        }
    }
}

data class MenuOption(val text: String, val icon: ImageVector)

@Composable
fun IconDropdownExample() {
    var expanded by remember { mutableStateOf(false) }
    var selectedOption by remember { mutableStateOf("Menu") }
    
    val options = listOf(
        MenuOption("Home", Icons.Default.Home),
        MenuOption("Settings", Icons.Default.Settings),
        MenuOption("Profile", Icons.Default.Person),
        MenuOption("Logout", Icons.Default.ExitToApp)
    )
    
    Column(modifier = Modifier.padding(16.dp)) {
        Button(onClick = { expanded = true }) {
            Text(selectedOption)
        }
        
        DropdownMenu(
            expanded = expanded,
            onDismissRequest = { expanded = false }
        ) {
            options.forEach { option ->
                DropdownMenuItem(
                    text = { Text(option.text) },
                    leadingIcon = {
                        Icon(option.icon, contentDescription = null)
                    },
                    onClick = {
                        selectedOption = option.text
                        expanded = false
                    }
                )
            }
        }
    }
}
```

---

## 🔄 Filterable Dropdown

```kotlin
package com.example.dropdowns

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class FilterableDropdownActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                FilterableDropdownExample()
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun FilterableDropdownExample() {
    var expanded by remember { mutableStateOf(false) }
    var searchText by remember { mutableStateOf("") }
    
    val allCountries = listOf(
        "USA", "Canada", "Mexico", "Brazil", "Argentina",
        "UK", "France", "Germany", "Italy", "Spain"
    )
    
    val filteredCountries = allCountries.filter {
        it.contains(searchText, ignoreCase = true)
    }
    
    Column(modifier = Modifier.padding(16.dp)) {
        ExposedDropdownMenuBox(
            expanded = expanded,
            onExpandedChange = { expanded = it }
        ) {
            TextField(
                value = searchText,
                onValueChange = { searchText = it },
                label = { Text("Select Country") },
                trailingIcon = { ExposedDropdownMenuDefaults.TrailingIcon(expanded = expanded) },
                modifier = Modifier.menuAnchor()
            )
            
            ExposedDropdownMenu(
                expanded = expanded,
                onDismissRequest = { expanded = false }
            ) {
                if (filteredCountries.isEmpty()) {
                    DropdownMenuItem(
                        text = { Text("No results found") },
                        onClick = { }
                    )
                } else {
                    filteredCountries.forEach { country ->
                        DropdownMenuItem(
                            text = { Text(country) },
                            onClick = {
                                searchText = country
                                expanded = false
                            }
                        )
                    }
                }
            }
        }
    }
}
```

---

## 🎯 Complete Example - Multi-Field Form

```kotlin
package com.example.dropdowns

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class FormWithDropdownActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                FormWithDropdown()
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun FormWithDropdown() {
    var name by remember { mutableStateOf("") }
    var country by remember { mutableStateOf("") }
    var countryExpanded by remember { mutableStateOf(false) }
    var age by remember { mutableStateOf("") }
    var ageExpanded by remember { mutableStateOf(false) }
    
    val countries = listOf("USA", "Canada", "UK", "Australia", "India")
    val ageRanges = listOf("18-25", "26-35", "36-45", "46-55", "56+")
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text("Registration Form", style = MaterialTheme.typography.headlineMedium)
        
        // Name field
        OutlinedTextField(
            value = name,
            onValueChange = { name = it },
            label = { Text("Full Name") },
            modifier = Modifier.fillMaxWidth()
        )
        
        // Country dropdown
        ExposedDropdownMenuBox(
            expanded = countryExpanded,
            onExpandedChange = { countryExpanded = it }
        ) {
            TextField(
                value = country,
                onValueChange = {},
                readOnly = true,
                label = { Text("Country") },
                trailingIcon = { ExposedDropdownMenuDefaults.TrailingIcon(expanded = countryExpanded) },
                modifier = Modifier.menuAnchor().fillMaxWidth()
            )
            
            ExposedDropdownMenu(
                expanded = countryExpanded,
                onDismissRequest = { countryExpanded = false }
            ) {
                countries.forEach { option ->
                    DropdownMenuItem(
                        text = { Text(option) },
                        onClick = {
                            country = option
                            countryExpanded = false
                        }
                    )
                }
            }
        }
        
        // Age dropdown
        ExposedDropdownMenuBox(
            expanded = ageExpanded,
            onExpandedChange = { ageExpanded = it }
        ) {
            TextField(
                value = age,
                onValueChange = {},
                readOnly = true,
                label = { Text("Age Range") },
                trailingIcon = { ExposedDropdownMenuDefaults.TrailingIcon(expanded = ageExpanded) },
                modifier = Modifier.menuAnchor().fillMaxWidth()
            )
            
            ExposedDropdownMenu(
                expanded = ageExpanded,
                onDismissRequest = { ageExpanded = false }
            ) {
                ageRanges.forEach { option ->
                    DropdownMenuItem(
                        text = { Text(option) },
                        onClick = {
                            age = option
                            ageExpanded = false
                        }
                    )
                }
            }
        }
        
        Button(
            onClick = { println("Submit: $name, $country, $age") },
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("Submit")
        }
    }
}
```

---

## ❓ Frequently Asked Questions

### Q: What's the difference between DropdownMenu and ExposedDropdownMenuBox?
**A:** 
- **DropdownMenu**: Basic dropdown that needs a trigger (like a Button)
- **ExposedDropdownMenuBox**: Material Design compliant with built-in TextField and arrow icon

### Q: Why do I need `@OptIn(ExperimentalMaterial3Api::class)`?
**A:** `ExposedDropdownMenuBox` is still experimental in Material3. This annotation acknowledges you're using an API that might change.

### Q: How do I make the dropdown full width?
**A:** Add `modifier = Modifier.fillMaxWidth()` to the TextField inside ExposedDropdownMenuBox.

### Q: Can users type in the dropdown?
**A:** 
- With `readOnly = true`: No, they can only select from the list
- With `readOnly = false`: Yes, you can create a filterable/searchable dropdown

### Q: How do I pre-select a value?
**A:** Initialize your state variable with a default value:
```kotlin
var selectedText by remember { mutableStateOf("Apple") }
```

### Q: How many items can a dropdown have?
**A:** No hard limit, but for very long lists (100+ items), consider using a searchable dialog or LazyColumn instead.

---

## 🔧 Quick Reference

| Component | Purpose | Key Properties | When to Use |
|-----------|---------|----------------|-------------|
| `DropdownMenu` | Basic menu | expanded, onDismissRequest | Simple menus, context menus |
| `ExposedDropdownMenuBox` | Material dropdown | expanded, onExpandedChange | Form inputs, settings |
| `DropdownMenuItem` | Menu item | text, onClick, leadingIcon | Individual options |
| `menuAnchor()` | Anchors menu | - | Required on TextField |

---

## 💡 Common Patterns

### Pattern 1: Read-only Dropdown (Most Common)
```kotlin
TextField(
    value = selected,
    onValueChange = {},
    readOnly = true,  // User can only select, not type
    // ...
)
```

### Pattern 2: Searchable Dropdown
```kotlin
TextField(
    value = searchText,
    onValueChange = { searchText = it },  // Allow typing
    readOnly = false,  // User can type to filter
    // ...
)
```

### Pattern 3: Dropdown with Sections
```kotlin
DropdownMenu(...) {
    Text("Recent", modifier = Modifier.padding(8.dp))
    HorizontalDivider()
    // Recent items...
    Text("All", modifier = Modifier.padding(8.dp))
    HorizontalDivider()
    // All items...
}
```

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
Once comfortable with dropdowns, explore **Dialog**, **BottomSheet**, and **AutoComplete** for more advanced selection pattern