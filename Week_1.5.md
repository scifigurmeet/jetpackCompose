# 🎨 Theming - Short & Sweet Guide

## 🎯 Quick Overview
**Theming** = Make your app look consistent and beautiful across all screens

| Component | Purpose | Key Properties |
|-----------|---------|----------------|
| `MaterialTheme` | App-wide theme wrapper | colorScheme, typography, shapes |
| `ColorScheme` | App colors (primary, background, etc.) | lightColorScheme(), darkColorScheme() |
| `dynamicColorScheme()` | Android 12+ adaptive colors | Follows system wallpaper |
| `isSystemInDarkTheme()` | Detect system theme | Auto light/dark switching |

---

## 🌈 Basic Material Theme Setup

```kotlin
package com.example.theming

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class BasicThemeActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MyAppTheme {
                Surface(modifier = Modifier.fillMaxSize()) {
                    ThemeDemo()
                }
            }
        }
    }
}

// Your app's theme
@Composable
fun MyAppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    val colorScheme = if (darkTheme) {
        darkColorScheme()  // Built-in dark colors
    } else {
        lightColorScheme() // Built-in light colors
    }
    
    MaterialTheme(
        colorScheme = colorScheme,
        content = content
    )
}

@Composable
fun ThemeDemo() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(20.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text(
            "Material Theme Demo",
            style = MaterialTheme.typography.headlineMedium,
            color = MaterialTheme.colorScheme.primary
        )
        
        Card {
            Text(
                "This card adapts to light/dark theme automatically!",
                modifier = Modifier.padding(16.dp)
            )
        }
        
        Button(onClick = {}) {
            Text("Primary Button")
        }
        
        OutlinedButton(onClick = {}) {
            Text("Outlined Button")
        }
    }
}

@Preview(showBackground = true)
@Preview(showBackground = true, uiMode = android.content.res.Configuration.UI_MODE_NIGHT_YES)
@Composable
fun ThemeDemoPreview() {
    MyAppTheme { ThemeDemo() }
}
```

---

## 🌙 Manual Theme Toggle

```kotlin
package com.example.theming

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.DarkMode
import androidx.compose.material.icons.filled.LightMode
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class ThemeToggleActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            ThemeToggleApp()
        }
    }
}

@Composable
fun ThemeToggleApp() {
    var isDarkTheme by remember { mutableStateOf(false) }
    
    MyAppTheme(darkTheme = isDarkTheme) {
        Surface(modifier = Modifier.fillMaxSize()) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(20.dp),
                horizontalAlignment = Alignment.CenterHorizontally,
                verticalArrangement = Arrangement.spacedBy(20.dp)
            ) {
                Text(
                    "Theme Toggle Demo",
                    style = MaterialTheme.typography.headlineMedium,
                    fontWeight = FontWeight.Bold
                )
                
                // Theme toggle button
                Card {
                    Row(
                        modifier = Modifier.padding(16.dp),
                        verticalAlignment = Alignment.CenterVertically,
                        horizontalArrangement = Arrangement.spacedBy(12.dp)
                    ) {
                        Icon(
                            if (isDarkTheme) Icons.Default.DarkMode else Icons.Default.LightMode,
                            contentDescription = null
                        )
                        
                        Text("${if (isDarkTheme) "Dark" else "Light"} Theme")
                        
                        Switch(
                            checked = isDarkTheme,
                            onCheckedChange = { isDarkTheme = it }
                        )
                    }
                }
                
                // Show different themed components
                Row(horizontalArrangement = Arrangement.spacedBy(12.dp)) {
                    Button(onClick = {}) { Text("Primary") }
                    OutlinedButton(onClick = {}) { Text("Secondary") }
                }
                
                Card(
                    colors = CardDefaults.cardColors(
                        containerColor = MaterialTheme.colorScheme.primaryContainer
                    )
                ) {
                    Text(
                        "Current theme colors adapt automatically!",
                        modifier = Modifier.padding(16.dp),
                        color = MaterialTheme.colorScheme.onPrimaryContainer
                    )
                }
            }
        }
    }
}

@Preview
@Composable
fun ThemeTogglePreview() {
    ThemeToggleApp()
}
```

---

## 🎨 Custom Color Schemes

```kotlin
package com.example.theming

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class CustomColorsActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            CustomColorApp()
        }
    }
}

// Define custom colors
val Purple = Color(0xFF6650a4)
val PurpleGrey = Color(0xFF625b71)
val Pink = Color(0xFF7D5260)
val Orange = Color(0xFFFF9800)
val Teal = Color(0xFF009688)

// Custom light color scheme
val CustomLightColors = lightColorScheme(
    primary = Purple,
    onPrimary = Color.White,
    primaryContainer = Color(0xFFEADDFF),
    onPrimaryContainer = Color(0xFF21005D),
    secondary = PurpleGrey,
    tertiary = Pink,
    background = Color(0xFFFFFBFE),
    surface = Color(0xFFFFFBFE),
    error = Color(0xFFBA1A1A),
)

// Custom dark color scheme
val CustomDarkColors = darkColorScheme(
    primary = Color(0xFFD0BCFF),
    onPrimary = Color(0xFF371E73),
    primaryContainer = Color(0xFF4F378B),
    onPrimaryContainer = Color(0xFFEADDFF),
    secondary = Color(0xFFCCC2DC),
    tertiary = Color(0xFFEFB8C8),
    background = Color(0xFF1C1B1F),
    surface = Color(0xFF1C1B1F),
    error = Color(0xFFF2B8B5),
)

@Composable
fun CustomColorTheme(
    darkTheme: Boolean = false,
    content: @Composable () -> Unit
) {
    val colorScheme = if (darkTheme) CustomDarkColors else CustomLightColors
    
    MaterialTheme(
        colorScheme = colorScheme,
        content = content
    )
}

@Composable
fun CustomColorApp() {
    var isDarkTheme by remember { mutableStateOf(false) }
    
    CustomColorTheme(darkTheme = isDarkTheme) {
        Surface(modifier = Modifier.fillMaxSize()) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(20.dp),
                verticalArrangement = Arrangement.spacedBy(16.dp)
            ) {
                // Header
                Text(
                    "Custom Colors",
                    style = MaterialTheme.typography.headlineMedium,
                    color = MaterialTheme.colorScheme.primary,
                    fontWeight = FontWeight.Bold
                )
                
                // Theme toggle
                Row(
                    verticalAlignment = Alignment.CenterVertically,
                    horizontalArrangement = Arrangement.spacedBy(8.dp)
                ) {
                    Text("Dark Theme")
                    Switch(
                        checked = isDarkTheme,
                        onCheckedChange = { isDarkTheme = it }
                    )
                }
                
                // Color showcase
                ColorShowcase()
            }
        }
    }
}

@Composable
fun ColorShowcase() {
    Column(verticalArrangement = Arrangement.spacedBy(12.dp)) {
        // Primary colors
        ColorCard(
            "Primary",
            MaterialTheme.colorScheme.primary,
            MaterialTheme.colorScheme.onPrimary
        )
        
        // Secondary colors  
        ColorCard(
            "Secondary",
            MaterialTheme.colorScheme.secondary,
            MaterialTheme.colorScheme.onSecondary
        )
        
        // Surface colors
        ColorCard(
            "Surface",
            MaterialTheme.colorScheme.surface,
            MaterialTheme.colorScheme.onSurface
        )
        
        // Container colors
        ColorCard(
            "Primary Container",
            MaterialTheme.colorScheme.primaryContainer,
            MaterialTheme.colorScheme.onPrimaryContainer
        )
        
        // Buttons showcase
        Row(horizontalArrangement = Arrangement.spacedBy(12.dp)) {
            Button(onClick = {}) { Text("Primary") }
            OutlinedButton(onClick = {}) { Text("Outlined") }
            TextButton(onClick = {}) { Text("Text") }
        }
    }
}

@Composable
fun ColorCard(name: String, backgroundColor: Color, contentColor: Color) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .background(backgroundColor, RoundedCornerShape(8.dp)),
        colors = CardDefaults.cardColors(containerColor = backgroundColor)
    ) {
        Text(
            text = name,
            modifier = Modifier.padding(16.dp),
            color = contentColor,
            fontWeight = FontWeight.Medium
        )
    }
}

@Preview
@Preview(uiMode = android.content.res.Configuration.UI_MODE_NIGHT_YES)
@Composable
fun CustomColorPreview() {
    CustomColorApp()
}
```

---

## 📱 Dynamic Colors (Android 12+)

```kotlin
package com.example.theming

import android.os.Build
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class DynamicThemeActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            DynamicThemeApp()
        }
    }
}

@Composable
fun DynamicThemeApp() {
    var useDynamicColors by remember { mutableStateOf(true) }
    val darkTheme = isSystemInDarkTheme()
    
    AppTheme(useDynamicColors = useDynamicColors, darkTheme = darkTheme) {
        Surface(modifier = Modifier.fillMaxSize()) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(20.dp),
                verticalArrangement = Arrangement.spacedBy(16.dp)
            ) {
                Text(
                    "Dynamic Colors Demo",
                    style = MaterialTheme.typography.headlineMedium,
                    fontWeight = FontWeight.Bold,
                    color = MaterialTheme.colorScheme.primary
                )
                
                // Feature availability
                val supportsDynamicColors = Build.VERSION.SDK_INT >= Build.VERSION_CODES.S
                
                Card(
                    colors = CardDefaults.cardColors(
                        containerColor = if (supportsDynamicColors) 
                            MaterialTheme.colorScheme.primaryContainer 
                        else MaterialTheme.colorScheme.errorContainer
                    )
                ) {
                    Column(modifier = Modifier.padding(16.dp)) {
                        Text(
                            "Dynamic Colors: ${if (supportsDynamicColors) "✅ Available" else "❌ Not Available"}",
                            fontWeight = FontWeight.Bold
                        )
                        Text(
                            if (supportsDynamicColors) 
                                "Colors adapt to your wallpaper!" 
                            else "Requires Android 12+ (API 31)"
                        )
                    }
                }
                
                // Toggle dynamic colors
                if (supportsDynamicColors) {
                    Card {
                        Row(
                            modifier = Modifier.padding(16.dp),
                            verticalAlignment = Alignment.CenterVertically,
                            horizontalArrangement = Arrangement.spacedBy(12.dp)
                        ) {
                            Text("Use Dynamic Colors")
                            Spacer(Modifier.weight(1f))
                            Switch(
                                checked = useDynamicColors,
                                onCheckedChange = { useDynamicColors = it }
                            )
                        }
                    }
                }
                
                // Show themed components
                Row(horizontalArrangement = Arrangement.spacedBy(12.dp)) {
                    Button(onClick = {}) { Text("Primary") }
                    FilledTonalButton(onClick = {}) { Text("Tonal") }
                    OutlinedButton(onClick = {}) { Text("Outlined") }
                }
                
                Card(
                    colors = CardDefaults.cardColors(
                        containerColor = MaterialTheme.colorScheme.secondaryContainer
                    )
                ) {
                    Text(
                        "Theme: ${if (useDynamicColors && supportsDynamicColors) "Dynamic" else "Custom"}",
                        modifier = Modifier.padding(16.dp),
                        color = MaterialTheme.colorScheme.onSecondaryContainer
                    )
                }
            }
        }
    }
}

@Composable
fun AppTheme(
    useDynamicColors: Boolean = true,
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    val context = LocalContext.current
    
    val colorScheme = when {
        useDynamicColors && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }
        darkTheme -> CustomDarkColors
        else -> CustomLightColors
    }
    
    MaterialTheme(
        colorScheme = colorScheme,
        content = content
    )
}

@Preview
@Composable
fun DynamicThemePreview() {
    DynamicThemeApp()
}
```

---

## 🎨 Complete Theme System

```kotlin
package com.example.theming

import android.os.Build
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class CompleteThemeActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            CompleteThemeDemo()
        }
    }
}

@Composable
fun CompleteThemeDemo() {
    var themeMode by remember { mutableStateOf(ThemeMode.SYSTEM) }
    val systemDarkTheme = isSystemInDarkTheme()
    
    val isDarkTheme = when (themeMode) {
        ThemeMode.LIGHT -> false
        ThemeMode.DARK -> true  
        ThemeMode.SYSTEM -> systemDarkTheme
    }
    
    MyCompleteTheme(darkTheme = isDarkTheme) {
        Surface(modifier = Modifier.fillMaxSize()) {
            LazyColumn(
                modifier = Modifier.padding(20.dp),
                verticalArrangement = Arrangement.spacedBy(16.dp)
            ) {
                item {
                    Text(
                        "Complete Theme System",
                        style = MaterialTheme.typography.headlineMedium,
                        fontWeight = FontWeight.Bold,
                        color = MaterialTheme.colorScheme.primary
                    )
                }
                
                // Theme selector
                item {
                    ThemeSelector(
                        currentTheme = themeMode,
                        onThemeChange = { themeMode = it }
                    )
                }
                
                // Component showcase
                item { ComponentShowcase() }
            }
        }
    }
}

enum class ThemeMode { LIGHT, DARK, SYSTEM }

@Composable
fun ThemeSelector(
    currentTheme: ThemeMode,
    onThemeChange: (ThemeMode) -> Unit
) {
    Card {
        Column(modifier = Modifier.padding(16.dp)) {
            Text("Theme Mode", fontWeight = FontWeight.Bold)
            Spacer(Modifier.height(8.dp))
            
            ThemeMode.values().forEach { theme ->
                Row(
                    verticalAlignment = Alignment.CenterVertically,
                    modifier = Modifier.fillMaxWidth()
                ) {
                    RadioButton(
                        selected = currentTheme == theme,
                        onClick = { onThemeChange(theme) }
                    )
                    Text(
                        when (theme) {
                            ThemeMode.LIGHT -> "☀️ Light"
                            ThemeMode.DARK -> "🌙 Dark"
                            ThemeMode.SYSTEM -> "📱 System"
                        }
                    )
                }
            }
        }
    }
}

@Composable
fun ComponentShowcase() {
    Column(verticalArrangement = Arrangement.spacedBy(12.dp)) {
        Text("Component Showcase", fontWeight = FontWeight.Bold)
        
        // Buttons
        Row(horizontalArrangement = Arrangement.spacedBy(8.dp)) {
            Button(onClick = {}) { Text("Button") }
            OutlinedButton(onClick = {}) { Text("Outlined") }
            TextButton(onClick = {}) { Text("Text") }
        }
        
        // Cards with different colors
        Card { Text("Surface Card", modifier = Modifier.padding(12.dp)) }
        
        Card(
            colors = CardDefaults.cardColors(
                containerColor = MaterialTheme.colorScheme.primaryContainer
            )
        ) {
            Text(
                "Primary Container",
                modifier = Modifier.padding(12.dp),
                color = MaterialTheme.colorScheme.onPrimaryContainer
            )
        }
        
        Card(
            colors = CardDefaults.cardColors(
                containerColor = MaterialTheme.colorScheme.secondaryContainer  
            )
        ) {
            Text(
                "Secondary Container",
                modifier = Modifier.padding(12.dp),
                color = MaterialTheme.colorScheme.onSecondaryContainer
            )
        }
        
        // Input field
        var text by remember { mutableStateOf("") }
        OutlinedTextField(
            value = text,
            onValueChange = { text = it },
            label = { Text("Sample Input") },
            modifier = Modifier.fillMaxWidth()
        )
    }
}

@Composable
fun MyCompleteTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    val context = LocalContext.current
    val supportsDynamic = Build.VERSION.SDK_INT >= Build.VERSION_CODES.S
    
    val colorScheme = when {
        supportsDynamic -> {
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }
        darkTheme -> CustomDarkColors
        else -> CustomLightColors
    }
    
    MaterialTheme(
        colorScheme = colorScheme,
        content = content
    )
}

@Preview
@Preview(uiMode = android.content.res.Configuration.UI_MODE_NIGHT_YES)
@Composable  
fun CompleteThemePreview() {
    CompleteThemeDemo()
}
```

---

## ❓ Quick FAQs

**Q: What's the difference between lightColorScheme() and dynamicLightColorScheme()?**
A: `dynamicLightColorScheme()` adapts to user's wallpaper colors (Android 12+), `lightColorScheme()` uses fixed Material colors.

**Q: How do I make my own colors work with dark theme?**
A: Create both light and dark versions of your ColorScheme, switch based on `isSystemInDarkTheme()`.

**Q: What colors should I customize first?**
A: Start with `primary`, `primaryContainer`, `secondary`. These affect buttons, highlights, and cards.

**Q: How do I preview both light and dark themes?**
A: Use `@Preview` twice: one normal, one with `uiMode = Configuration.UI_MODE_NIGHT_YES`.

**Q: Can I use custom colors with dynamic theming?**
A: Yes! Dynamic colors only work on Android 12+, so provide custom colors as fallback.

---

## 🎯 Essential Color Properties

```kotlin
// Most important colors to customize:
val myColorScheme = lightColorScheme(
    primary = Color(0xFF6650a4),           // Main brand color
    onPrimary = Color.White,               // Text on primary
    primaryContainer = Color(0xFFEADDFF),  // Lighter primary (cards/chips)
    secondary = Color(0xFF625b71),         // Secondary accent
    background = Color(0xFFFFFBFE),        // Screen background
    surface = Color(0xFFFFFBFE),           // Card/dialog background  
    error = Color(0xFFBA1A1A),             // Error states
)
```

## 🎨 Quick Setup Template

```kotlin
// 1. Define your theme
@Composable
fun MyAppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    MaterialTheme(
        colorScheme = if (darkTheme) darkColorScheme() else lightColorScheme(),
        content = content
    )
}

// 2. Wrap your app
setContent {
    MyAppTheme {
        Surface(modifier = Modifier.fillMaxSize()) {
            MyAppContent()
        }
    }
}

// 3. Use theme colors
Text(
    "Hello World",
    color = MaterialTheme.colorScheme.primary
)
```

## 🔧 Required Dependencies
```kotlin
implementation "androidx.compose.material3:material3:1.1.2"
```

**🎯 Key Takeaway: Material Theme automatically handles light/dark switching - just wrap your app!**
