# 📚 Wikipedia Search App - Jetpack Compose

## 🎯 What We're Building
A simple app that searches Wikipedia and displays article extracts. Users enter a topic, click search, and see the result!

---

## 📦 Step 1: Add Dependencies

Open `build.gradle.kts` (Module: app) and add these:

```kotlin
dependencies {
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.6.2")
    implementation("androidx.activity:activity-compose:1.8.0")
    
    // Compose
    implementation(platform("androidx.compose:compose-bom:2023.10.01"))
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.ui:ui-graphics")
    implementation("androidx.compose.ui:ui-tooling-preview")
    implementation("androidx.compose.material3:material3")
    
    // Network - Retrofit & Gson
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    
    // Coroutines
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3")
    
    debugImplementation("androidx.compose.ui:ui-tooling")
}
```

**Then click "Sync Now"** in Android Studio.

---

## 🌐 Step 2: Add Internet Permission

Open `AndroidManifest.xml` and add this line **before** the `<application>` tag:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

---

## 💻 Step 3: Complete Working Code

Copy this entire code into your `MainActivity.kt`:

```kotlin
package com.example.wikipediasearch

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.rememberScrollState
import androidx.compose.foundation.verticalScroll
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Search
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import kotlinx.coroutines.launch
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import retrofit2.http.GET
import retrofit2.http.Query

// ==================== MAIN ACTIVITY ====================
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                WikipediaSearchApp()
            }
        }
    }
}

// ==================== DATA CLASSES ====================
// These represent the JSON structure from Wikipedia API

data class WikiResponse(
    val query: Query
)

data class Query(
    val pages: Map<String, Page>
)

data class Page(
    val pageid: Int,
    val title: String,
    val extract: String
)

// ==================== RETROFIT API ====================
// Interface for making network calls

interface WikipediaApi {
    @GET("api.php")
    suspend fun searchArticle(
        @Query("format") format: String = "json",
        @Query("action") action: String = "query",
        @Query("prop") prop: String = "extracts",
        @Query("exintro") exintro: String = "",
        @Query("explaintext") explaintext: String = "",
        @Query("redirects") redirects: Int = 1,
        @Query("titles") titles: String
    ): WikiResponse
}

// ==================== RETROFIT SETUP ====================
// Create the API service

object RetrofitInstance {
    private const val BASE_URL = "https://en.wikipedia.org/w/"
    
    val api: WikipediaApi by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(WikipediaApi::class.java)
    }
}

// ==================== UI COMPOSABLES ====================

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun WikipediaSearchApp() {
    // State variables
    var searchText by remember { mutableStateOf("") }
    var articleTitle by remember { mutableStateOf("") }
    var articleExtract by remember { mutableStateOf("") }
    var isLoading by remember { mutableStateOf(false) }
    var errorMessage by remember { mutableStateOf("") }
    
    // Coroutine scope for API calls
    val scope = rememberCoroutineScope()
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { 
                    Text(
                        "📚 Wikipedia Search",
                        fontWeight = FontWeight.Bold
                    ) 
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.primary
                )
            )
        }
    ) { innerPadding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(innerPadding)
                .padding(16.dp)
                .verticalScroll(rememberScrollState()),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            // ========== SEARCH SECTION ==========
            Card(
                modifier = Modifier.fillMaxWidth(),
                elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
            ) {
                Column(
                    modifier = Modifier.padding(16.dp),
                    verticalArrangement = Arrangement.spacedBy(12.dp)
                ) {
                    Text(
                        "🔍 Search Wikipedia",
                        style = MaterialTheme.typography.titleMedium,
                        fontWeight = FontWeight.SemiBold
                    )
                    
                    // Text Input Field
                    OutlinedTextField(
                        value = searchText,
                        onValueChange = { searchText = it },
                        label = { Text("Enter topic") },
                        placeholder = { Text("e.g., Ludhiana, India, Space...") },
                        modifier = Modifier.fillMaxWidth(),
                        singleLine = true,
                        enabled = !isLoading
                    )
                    
                    // Search Button
                    Button(
                        onClick = {
                            if (searchText.isNotBlank()) {
                                // Clear previous results
                                articleTitle = ""
                                articleExtract = ""
                                errorMessage = ""
                                isLoading = true
                                
                                // Make API call
                                scope.launch {
                                    try {
                                        val response = RetrofitInstance.api.searchArticle(
                                            titles = searchText
                                        )
                                        
                                        // Extract the first page from response
                                        val page = response.query.pages.values.firstOrNull()
                                        
                                        if (page != null && page.extract.isNotEmpty()) {
                                            articleTitle = page.title
                                            articleExtract = page.extract
                                        } else {
                                            errorMessage = "❌ No article found for '$searchText'"
                                        }
                                    } catch (e: Exception) {
                                        errorMessage = "⚠️ Error: ${e.message}"
                                    } finally {
                                        isLoading = false
                                    }
                                }
                            }
                        },
                        modifier = Modifier.fillMaxWidth(),
                        enabled = searchText.isNotBlank() && !isLoading
                    ) {
                        Icon(
                            Icons.Filled.Search,
                            contentDescription = null,
                            modifier = Modifier.size(20.dp)
                        )
                        Spacer(modifier = Modifier.width(8.dp))
                        Text(if (isLoading) "Searching..." else "Search")
                    }
                }
            }
            
            Spacer(modifier = Modifier.height(16.dp))
            
            // ========== LOADING INDICATOR ==========
            if (isLoading) {
                CircularProgressIndicator()
            }
            
            // ========== ERROR MESSAGE ==========
            if (errorMessage.isNotEmpty()) {
                Card(
                    modifier = Modifier.fillMaxWidth(),
                    colors = CardDefaults.cardColors(
                        containerColor = MaterialTheme.colorScheme.errorContainer
                    )
                ) {
                    Text(
                        text = errorMessage,
                        modifier = Modifier.padding(16.dp),
                        color = MaterialTheme.colorScheme.onErrorContainer
                    )
                }
            }
            
            // ========== ARTICLE RESULT ==========
            if (articleTitle.isNotEmpty() && articleExtract.isNotEmpty()) {
                Card(
                    modifier = Modifier.fillMaxWidth(),
                    elevation = CardDefaults.cardElevation(defaultElevation = 2.dp)
                ) {
                    Column(
                        modifier = Modifier.padding(16.dp),
                        verticalArrangement = Arrangement.spacedBy(12.dp)
                    ) {
                        // Article Title
                        Text(
                            text = articleTitle,
                            style = MaterialTheme.typography.headlineSmall,
                            fontWeight = FontWeight.Bold,
                            color = MaterialTheme.colorScheme.primary
                        )
                        
                        Divider()
                        
                        // Article Extract
                        Text(
                            text = articleExtract,
                            style = MaterialTheme.typography.bodyMedium,
                            lineHeight = MaterialTheme.typography.bodyMedium.lineHeight
                        )
                    }
                }
            }
            
            // ========== HELP TEXT ==========
            if (articleTitle.isEmpty() && !isLoading && errorMessage.isEmpty()) {
                Spacer(modifier = Modifier.height(24.dp))
                Text(
                    "💡 Enter a topic above and click Search to get information from Wikipedia",
                    style = MaterialTheme.typography.bodySmall,
                    color = MaterialTheme.colorScheme.onSurfaceVariant
                )
            }
        }
    }
}
```

---

## 🔍 Code Explanation

### 📊 Data Classes (Lines 29-42)
```kotlin
data class WikiResponse(val query: Query)
data class Query(val pages: Map<String, Page>)
data class Page(val pageid: Int, val title: String, val extract: String)
```
- **What**: These represent the JSON structure from Wikipedia
- **Why**: Kotlin needs these to convert JSON into objects

### 🌐 Retrofit Interface (Lines 44-57)
```kotlin
interface WikipediaApi {
    @GET("api.php")
    suspend fun searchArticle(...)
}
```
- **What**: Defines how to call the Wikipedia API
- **`@GET`**: HTTP GET method
- **`@Query`**: URL parameters (format, action, titles, etc.)
- **`suspend`**: Can be called from coroutines (async)

### 🏗️ Retrofit Instance (Lines 59-72)
```kotlin
object RetrofitInstance {
    val api: WikipediaApi by lazy { ... }
}
```
- **What**: Creates the API service
- **`lazy`**: Only creates it when first used
- **BASE_URL**: Wikipedia's API base address

### 🎨 UI Components

#### State Variables (Lines 79-84)
```kotlin
var searchText by remember { mutableStateOf("") }
var isLoading by remember { mutableStateOf(false) }
```
- **`remember`**: Keeps values across screen updates
- **`mutableStateOf`**: Value that triggers UI updates when changed

#### Coroutine Scope (Line 87)
```kotlin
val scope = rememberCoroutineScope()
```
- **What**: Allows running background tasks (API calls)
- **Why**: Network calls can't run on main thread

#### Scaffold (Line 89)
```kotlin
Scaffold(topBar = { ... }) { ... }
```
- **What**: Basic screen structure
- **`topBar`**: The toolbar at top
- **`innerPadding`**: Ensures content doesn't overlap toolbar

#### API Call Logic (Lines 130-154)
```kotlin
scope.launch {
    try {
        val response = RetrofitInstance.api.searchArticle(titles = searchText)
        val page = response.query.pages.values.firstOrNull()
        // ... process response
    } catch (e: Exception) {
        errorMessage = "Error: ${e.message}"
    } finally {
        isLoading = false
    }
}
```
- **`launch`**: Starts background task
- **`try-catch`**: Handles errors gracefully
- **`finally`**: Always runs (stops loading indicator)

---

## 🎯 How It Works: Step-by-Step

1. **User enters text** → Updates `searchText` state
2. **User clicks Search** → Button's `onClick` triggers
3. **`scope.launch`** → Starts background task
4. **`isLoading = true`** → Shows loading spinner
5. **API call made** → Retrofit sends HTTP request to Wikipedia
6. **Response received** → JSON converted to Kotlin objects
7. **Extract data** → Gets title and extract from response
8. **Update UI** → Sets `articleTitle` and `articleExtract`
9. **UI recomposes** → Shows the article automatically
10. **`isLoading = false`** → Hides spinner

---

## 📱 What Each UI Component Does

### OutlinedTextField
```kotlin
OutlinedTextField(
    value = searchText,
    onValueChange = { searchText = it }
)
```
- **Purpose**: Text input box
- **`value`**: Current text
- **`onValueChange`**: Called when user types

### Button
```kotlin
Button(
    onClick = { /* API call */ },
    enabled = searchText.isNotBlank() && !isLoading
)
```
- **Purpose**: Triggers search
- **`enabled`**: Only works if text entered and not loading

### CircularProgressIndicator
```kotlin
if (isLoading) {
    CircularProgressIndicator()
}
```
- **Purpose**: Shows spinning loader while waiting

### Card
```kotlin
Card(elevation = CardDefaults.cardElevation(...)) {
    // Content
}
```
- **Purpose**: Container with shadow/elevation
- **Makes**: Content look raised/professional

---

## 🚀 Running the App

1. **Copy all code** into `MainActivity.kt`
2. **Add dependencies** to `build.gradle.kts`
3. **Add internet permission** to `AndroidManifest.xml`
4. **Click "Sync Now"** in Android Studio
5. **Run the app** on emulator or device
6. **Type a topic** (e.g., "Python", "India", "Moon")
7. **Click Search** and see the result!

---

## 🎨 Key Concepts Learned

### ✅ State Management
- `remember` keeps values during recomposition
- `mutableStateOf` triggers UI updates

### ✅ Network Calls
- Retrofit for API calls
- Coroutines for background tasks
- JSON parsing with Gson

### ✅ UI Composition
- Scaffold for app structure
- Cards for grouped content
- Material 3 components

### ✅ Error Handling
- Try-catch for network errors
- Loading states
- User feedback

---

## 🎓 Try These Modifications

1. **Add Clear Button**: Clear search text
2. **Save History**: Remember previous searches
3. **Change Language**: Search in different Wikipedia languages
4. **Add Images**: Fetch and show article images
5. **Share Article**: Add button to share text

---

## 📚 API Explanation

**Wikipedia API URL Structure:**
```
https://en.wikipedia.org/w/api.php
  ?format=json           ← Return JSON
  &action=query          ← Query action
  &prop=extracts         ← Get article extract
  &exintro=              ← Only intro section
  &explaintext=          ← Plain text (no HTML)
  &redirects=1           ← Follow redirects
  &titles=Ludhiana       ← Search term
```

---

## 🐛 Common Issues & Fixes

### ❌ "Unable to resolve dependency"
**Fix**: Make sure you clicked "Sync Now" after adding dependencies

### ❌ "CLEARTEXT communication not permitted"
**Fix**: Add internet permission in `AndroidManifest.xml`

### ❌ App crashes on search
**Fix**: Check internet connection and API URL

### ❌ "No article found"
**Fix**: Try different spelling or broader terms

---

## 🎉 Congratulations!

You've built a real app that:
- ✅ Takes user input
- ✅ Calls a real API
- ✅ Parses JSON
- ✅ Displays results
- ✅ Handles errors
- ✅ Shows loading states

**This is a complete, production-ready pattern!** 🚀