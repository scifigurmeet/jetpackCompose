# 📱 Jetpack Compose: Toolbar & Menu Options Guide

## 🎯 What You'll Learn
- Creating Top App Bars (Toolbars)
- Adding Menu Items with Icons
- Handling Menu Actions
- Creating Dropdown Menus
- Bottom App Bars

---

## 📚 Table of Contents
1. [Basic Top App Bar](#1-basic-top-app-bar)
2. [Top App Bar with Menu Icons](#2-top-app-bar-with-menu-icons)
3. [Dropdown Menu (Overflow Menu)](#3-dropdown-menu-overflow-menu)
4. [Complete Example with All Features](#4-complete-example-with-all-features)
5. [Bottom App Bar](#5-bottom-app-bar)

---

## 1️⃣ Basic Top App Bar

### 📖 What is a Top App Bar?
A Top App Bar (formerly called Toolbar) is the horizontal bar at the top of your app that displays the app title, navigation icon, and actions.

### 💻 Complete Working Code

```kotlin
package com.example.myapp

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ArrowBack
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                BasicTopAppBarExample()
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun BasicTopAppBarExample() {
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("My App") },
                navigationIcon = {
                    IconButton(onClick = { /* Handle back press */ }) {
                        Icon(Icons.Filled.ArrowBack, contentDescription = "Back")
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.primary,
                )
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .padding(innerPadding)
                .fillMaxSize()
        ) {
            Text("Hello, World!", modifier = Modifier.padding(16.dp))
        }
    }
}

@Preview(showBackground = true)
@Composable
fun BasicPreview() {
    MaterialTheme {
        BasicTopAppBarExample()
    }
}
```

### 🔍 Explanation
- **`Scaffold`**: Container that provides structure with top bar, bottom bar, and content
- **`TopAppBar`**: The actual toolbar component
- **`title`**: Text displayed in the toolbar
- **`navigationIcon`**: Icon on the left (usually back button)
- **`innerPadding`**: Ensures content doesn't overlap with the toolbar

---

## 2️⃣ Top App Bar with Menu Icons

### 📖 What are Action Icons?
Action icons are buttons in the toolbar that trigger immediate actions (like search, share, etc.)

### 💻 Complete Working Code

```kotlin
package com.example.myapp

import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                TopAppBarWithActions()
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun TopAppBarWithActions() {
    val context = LocalContext.current
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("My App") },
                navigationIcon = {
                    IconButton(onClick = {
                        Toast.makeText(context, "Menu Clicked", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Menu, contentDescription = "Menu")
                    }
                },
                actions = {
                    // Search Icon
                    IconButton(onClick = {
                        Toast.makeText(context, "Search Clicked", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Search, contentDescription = "Search")
                    }
                    
                    // Favorite Icon
                    IconButton(onClick = {
                        Toast.makeText(context, "Favorite Clicked", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Favorite, contentDescription = "Favorite")
                    }
                    
                    // Share Icon
                    IconButton(onClick = {
                        Toast.makeText(context, "Share Clicked", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Share, contentDescription = "Share")
                    }
                }
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .padding(innerPadding)
                .fillMaxSize()
                .padding(16.dp)
        ) {
            Text("Click toolbar icons to see actions!", style = MaterialTheme.typography.bodyLarge)
        }
    }
}

@Preview(showBackground = true)
@Composable
fun ActionsPreview() {
    MaterialTheme {
        TopAppBarWithActions()
    }
}
```

### 🔍 Explanation
- **`actions`**: Lambda where you place action icons (right side of toolbar)
- **`IconButton`**: Clickable icon with onClick handler
- **`Toast`**: Shows brief messages to user
- **`LocalContext.current`**: Gets Android context for Toast

---

## 3️⃣ Dropdown Menu (Overflow Menu)

### 📖 What is a Dropdown Menu?
The three-dot menu that shows more options when clicked. Perfect for less frequently used actions.

### 💻 Complete Working Code

```kotlin
package com.example.myapp

import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                TopAppBarWithDropdownMenu()
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun TopAppBarWithDropdownMenu() {
    val context = LocalContext.current
    var showMenu by remember { mutableStateOf(false) }
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Dropdown Menu Demo") },
                navigationIcon = {
                    IconButton(onClick = {
                        Toast.makeText(context, "Back", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.ArrowBack, contentDescription = "Back")
                    }
                },
                actions = {
                    // Search Icon
                    IconButton(onClick = {
                        Toast.makeText(context, "Search", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Search, contentDescription = "Search")
                    }
                    
                    // More Options (3 dots)
                    Box {
                        IconButton(onClick = { showMenu = !showMenu }) {
                            Icon(Icons.Filled.MoreVert, contentDescription = "More Options")
                        }
                        
                        DropdownMenu(
                            expanded = showMenu,
                            onDismissRequest = { showMenu = false }
                        ) {
                            DropdownMenuItem(
                                text = { Text("Settings") },
                                onClick = {
                                    Toast.makeText(context, "Settings", Toast.LENGTH_SHORT).show()
                                    showMenu = false
                                },
                                leadingIcon = {
                                    Icon(Icons.Filled.Settings, contentDescription = null)
                                }
                            )
                            
                            DropdownMenuItem(
                                text = { Text("Help") },
                                onClick = {
                                    Toast.makeText(context, "Help", Toast.LENGTH_SHORT).show()
                                    showMenu = false
                                },
                                leadingIcon = {
                                    Icon(Icons.Filled.Info, contentDescription = null)
                                }
                            )
                            
                            Divider()
                            
                            DropdownMenuItem(
                                text = { Text("Logout") },
                                onClick = {
                                    Toast.makeText(context, "Logout", Toast.LENGTH_SHORT).show()
                                    showMenu = false
                                },
                                leadingIcon = {
                                    Icon(Icons.Filled.ExitToApp, contentDescription = null)
                                }
                            )
                        }
                    }
                }
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .padding(innerPadding)
                .fillMaxSize()
                .padding(16.dp)
        ) {
            Text(
                "Click the three-dot menu to see options!",
                style = MaterialTheme.typography.bodyLarge
            )
        }
    }
}

@Preview(showBackground = true)
@Composable
fun DropdownPreview() {
    MaterialTheme {
        TopAppBarWithDropdownMenu()
    }
}
```

### 🔍 Explanation
- **`remember { mutableStateOf() }`**: Remembers state across recompositions
- **`showMenu`**: Boolean that controls menu visibility
- **`DropdownMenu`**: The popup menu container
- **`expanded`**: Controls if menu is shown
- **`onDismissRequest`**: Called when clicking outside menu
- **`DropdownMenuItem`**: Individual menu item
- **`leadingIcon`**: Icon before text in menu item
- **`Divider()`**: Visual separator between menu items

---

## 4️⃣ Complete Example with All Features

### 💻 Complete Working Code

```kotlin
package com.example.myapp

import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                CompleteToolbarExample()
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun CompleteToolbarExample() {
    val context = LocalContext.current
    var showMenu by remember { mutableStateOf(false) }
    var isFavorite by remember { mutableStateOf(false) }
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Complete Example") },
                navigationIcon = {
                    IconButton(onClick = {
                        Toast.makeText(context, "Navigation", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Menu, contentDescription = "Menu")
                    }
                },
                actions = {
                    // Search Action
                    IconButton(onClick = {
                        Toast.makeText(context, "Search", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Search, contentDescription = "Search")
                    }
                    
                    // Toggle Favorite
                    IconButton(onClick = {
                        isFavorite = !isFavorite
                        val message = if (isFavorite) "Added to favorites" else "Removed from favorites"
                        Toast.makeText(context, message, Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(
                            if (isFavorite) Icons.Filled.Favorite else Icons.Filled.FavoriteBorder,
                            contentDescription = "Favorite",
                            tint = if (isFavorite) MaterialTheme.colorScheme.error else LocalContentColor.current
                        )
                    }
                    
                    // More Options Menu
                    Box {
                        IconButton(onClick = { showMenu = !showMenu }) {
                            Icon(Icons.Filled.MoreVert, contentDescription = "More")
                        }
                        
                        DropdownMenu(
                            expanded = showMenu,
                            onDismissRequest = { showMenu = false }
                        ) {
                            DropdownMenuItem(
                                text = { Text("Share") },
                                onClick = {
                                    Toast.makeText(context, "Share", Toast.LENGTH_SHORT).show()
                                    showMenu = false
                                },
                                leadingIcon = { Icon(Icons.Filled.Share, contentDescription = null) }
                            )
                            
                            DropdownMenuItem(
                                text = { Text("Settings") },
                                onClick = {
                                    Toast.makeText(context, "Settings", Toast.LENGTH_SHORT).show()
                                    showMenu = false
                                },
                                leadingIcon = { Icon(Icons.Filled.Settings, contentDescription = null) }
                            )
                            
                            DropdownMenuItem(
                                text = { Text("About") },
                                onClick = {
                                    Toast.makeText(context, "About", Toast.LENGTH_SHORT).show()
                                    showMenu = false
                                },
                                leadingIcon = { Icon(Icons.Filled.Info, contentDescription = null) }
                            )
                            
                            Divider()
                            
                            DropdownMenuItem(
                                text = { Text("Logout") },
                                onClick = {
                                    Toast.makeText(context, "Logout", Toast.LENGTH_SHORT).show()
                                    showMenu = false
                                },
                                leadingIcon = { Icon(Icons.Filled.ExitToApp, contentDescription = null) }
                            )
                        }
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.primary,
                )
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .padding(innerPadding)
                .fillMaxSize()
                .padding(16.dp),
            verticalArrangement = Arrangement.spacedBy(12.dp)
        ) {
            Card(
                modifier = Modifier.fillMaxWidth()
            ) {
                Column(modifier = Modifier.padding(16.dp)) {
                    Text("🎯 Toolbar Features:", style = MaterialTheme.typography.titleMedium)
                    Spacer(modifier = Modifier.height(8.dp))
                    Text("✅ Navigation icon (left)")
                    Text("✅ Search action")
                    Text("✅ Toggle favorite with state")
                    Text("✅ Dropdown menu with options")
                    Text("✅ Custom colors")
                }
            }
            
            Text(
                "Favorite Status: ${if (isFavorite) "❤️ Favorited" else "🤍 Not Favorited"}",
                style = MaterialTheme.typography.bodyLarge
            )
        }
    }
}

@Preview(showBackground = true)
@Composable
fun CompletePreview() {
    MaterialTheme {
        CompleteToolbarExample()
    }
}
```

### 🔍 Key Features
- **Stateful Icon**: Favorite icon changes based on state
- **Color Tinting**: Red heart when favorited
- **Multiple Actions**: Combines direct actions with overflow menu
- **Custom Styling**: Custom colors for toolbar

---

## 5️⃣ Bottom App Bar

### 📖 What is a Bottom App Bar?
An app bar positioned at the bottom, often used for primary actions in mobile apps.

### 💻 Complete Working Code

```kotlin
package com.example.myapp

import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                BottomAppBarExample()
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun BottomAppBarExample() {
    val context = LocalContext.current
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Bottom App Bar Demo") }
            )
        },
        bottomBar = {
            BottomAppBar(
                actions = {
                    IconButton(onClick = {
                        Toast.makeText(context, "Home", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Home, contentDescription = "Home")
                    }
                    
                    IconButton(onClick = {
                        Toast.makeText(context, "Search", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Search, contentDescription = "Search")
                    }
                    
                    IconButton(onClick = {
                        Toast.makeText(context, "Notifications", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Notifications, contentDescription = "Notifications")
                    }
                    
                    IconButton(onClick = {
                        Toast.makeText(context, "Settings", Toast.LENGTH_SHORT).show()
                    }) {
                        Icon(Icons.Filled.Settings, contentDescription = "Settings")
                    }
                },
                floatingActionButton = {
                    FloatingActionButton(
                        onClick = {
                            Toast.makeText(context, "Add New", Toast.LENGTH_SHORT).show()
                        },
                        containerColor = BottomAppBarDefaults.bottomAppBarFabColor,
                        elevation = FloatingActionButtonDefaults.bottomAppBarFabElevation()
                    ) {
                        Icon(Icons.Filled.Add, "Add")
                    }
                }
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .padding(innerPadding)
                .fillMaxSize()
                .padding(16.dp)
        ) {
            Text(
                "Bottom App Bar with FAB!",
                style = MaterialTheme.typography.headlineMedium
            )
            Spacer(modifier = Modifier.height(16.dp))
            Text("Actions are at the bottom of the screen.")
            Text("The floating action button (FAB) is prominent for primary action.")
        }
    }
}

@Preview(showBackground = true)
@Composable
fun BottomBarPreview() {
    MaterialTheme {
        BottomAppBarExample()
    }
}
```

### 🔍 Explanation
- **`BottomAppBar`**: Container for bottom toolbar
- **`floatingActionButton`**: Prominent button for primary action
- **`bottomAppBarFabColor`**: Special color for FAB in bottom bar

---

## 📦 Required Dependencies

Add these to your `build.gradle.kts` (Module: app):

```kotlin
dependencies {
    implementation("androidx.compose.ui:ui:1.5.4")
    implementation("androidx.compose.material3:material3:1.1.2")
    implementation("androidx.compose.ui:ui-tooling-preview:1.5.4")
    implementation("androidx.activity:activity-compose:1.8.0")
    
    debugImplementation("androidx.compose.ui:ui-tooling:1.5.4")
}
```

---

## 🎓 Quick Reference: Common Icons

```kotlin
// Navigation & Actions
Icons.Filled.Menu
Icons.Filled.ArrowBack
Icons.Filled.Close
Icons.Filled.Search
Icons.Filled.MoreVert

// Content Actions
Icons.Filled.Add
Icons.Filled.Edit
Icons.Filled.Delete
Icons.Filled.Share
Icons.Filled.Favorite
Icons.Filled.FavoriteBorder

// App Functions
Icons.Filled.Settings
Icons.Filled.Info
Icons.Filled.Home
Icons.Filled.Person
Icons.Filled.Notifications
Icons.Filled.ExitToApp
```

---

## 💡 Best Practices

1. **Limit Action Icons**: Show 2-3 most important actions in toolbar
2. **Use Overflow Menu**: Put less frequent actions in dropdown menu
3. **Provide Descriptions**: Always add `contentDescription` for accessibility
4. **Handle State**: Use `remember` and `mutableStateOf` for interactive elements
5. **Toast Feedback**: Show feedback when actions are performed
6. **Consistent Icons**: Use Material Icons for familiar UI

---

## 🚀 Challenge Exercises

Try these to practice:

1. **Create a Search Bar**: Make the search icon expand into a text field
2. **Add Badges**: Show notification count on notification icon
3. **Theme Toggle**: Add a dark/light mode toggle in menu
4. **Multi-Select**: Create a contextual action bar when items are selected
5. **Animated Icons**: Make favorite icon animate when clicked

---

## 📚 Additional Resources

- [Material 3 Compose Components](https://developer.android.com/jetpack/compose/designsystems/material3)
- [Material Icons Guide](https://fonts.google.com/icons)
- [Compose State Management](https://developer.android.com/jetpack/compose/state)

---

**Happy Coding! 🎉**