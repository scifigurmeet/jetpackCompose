# 📜 Jetpack Compose Lazy Lists - 1 Hour Study Guide

## 🎯 What You'll Learn (60 minutes)
- **LazyColumn** - Vertical scrolling lists
- **LazyRow** - Horizontal scrolling lists  
- **Performance benefits** over regular Column/Row
- **Real-world examples** with complete code

---

## 🚀 Why Lazy Lists?

### Problem with Regular Lists
```kotlin
// ❌ DON'T DO THIS for large lists
Column {
    repeat(1000) { index ->
        Text("Item $index")  // Creates ALL 1000 items in memory!
    }
}
```

### Solution: Lazy Lists
```kotlin
// ✅ DO THIS - Only visible items are created
LazyColumn {
    items(1000) { index ->
        Text("Item $index")  // Only visible items exist in memory!
    }
}
```

**🔑 Key Benefits:**
- **Memory efficient** - Only renders visible items
- **Smooth scrolling** - Better performance
- **Handle large datasets** - Thousands of items easily

---

## 📊 LazyColumn - Vertical Lists

### Basic LazyColumn

```kotlin
package com.example.lazylists

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp

class BasicLazyColumnActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    BasicLazyColumnExample()
                }
            }
        }
    }
}

@Composable
fun BasicLazyColumnExample() {
    val itemsList = (1..100).map { "Item $it" }
    
    LazyColumn(
        modifier = Modifier.fillMaxSize(),
        contentPadding = PaddingValues(16.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        items(itemsList) { item ->
            Card(
                modifier = Modifier.fillMaxWidth(),
                elevation = CardDefaults.cardElevation(defaultElevation = 2.dp)
            ) {
                Text(
                    text = item,
                    modifier = Modifier.padding(16.dp)
                )
            }
        }
    }
}

@Preview(showBackground = true)
@Composable
fun BasicLazyColumnPreview() {
    MaterialTheme {
        BasicLazyColumnExample()
    }
}
```

### Contact List Example

```kotlin
package com.example.lazylists

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.CircleShape
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Person
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

data class Contact(
    val name: String,
    val phone: String,
    val email: String
)

class ContactListActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    ContactListScreen()
                }
            }
        }
    }
}

@Composable
fun ContactListScreen() {
    val contacts = listOf(
        Contact("John Doe", "+1 234-567-8900", "john@email.com"),
        Contact("Jane Smith", "+1 234-567-8901", "jane@email.com"),
        Contact("Bob Johnson", "+1 234-567-8902", "bob@email.com"),
        Contact("Alice Brown", "+1 234-567-8903", "alice@email.com"),
        Contact("Charlie Wilson", "+1 234-567-8904", "charlie@email.com"),
        Contact("Diana Davis", "+1 234-567-8905", "diana@email.com"),
        Contact("Eve Miller", "+1 234-567-8906", "eve@email.com"),
        Contact("Frank Garcia", "+1 234-567-8907", "frank@email.com"),
        Contact("Grace Lee", "+1 234-567-8908", "grace@email.com"),
        Contact("Henry Taylor", "+1 234-567-8909", "henry@email.com")
    )
    
    LazyColumn(
        modifier = Modifier.fillMaxSize(),
        contentPadding = PaddingValues(16.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        item {
            Text(
                text = "Contacts (${contacts.size})",
                fontSize = 24.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier.padding(bottom = 8.dp)
            )
        }
        
        items(contacts) { contact ->
            ContactItem(contact = contact)
        }
    }
}

@Composable
fun ContactItem(contact: Contact) {
    Card(
        modifier = Modifier.fillMaxWidth(),
        elevation = CardDefaults.cardElevation(defaultElevation = 2.dp)
    ) {
        Row(
            modifier = Modifier.padding(16.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            // Profile picture placeholder
            Box(
                modifier = Modifier
                    .size(50.dp)
                    .background(Color.LightGray, CircleShape),
                contentAlignment = Alignment.Center
            ) {
                Image(
                    imageVector = Icons.Default.Person,
                    contentDescription = "Profile picture",
                    modifier = Modifier.size(30.dp)
                )
            }
            
            Spacer(modifier = Modifier.width(12.dp))
            
            Column(modifier = Modifier.weight(1f)) {
                Text(
                    text = contact.name,
                    fontSize = 16.sp,
                    fontWeight = FontWeight.Bold
                )
                Text(
                    text = contact.phone,
                    fontSize = 14.sp,
                    color = Color.Gray
                )
                Text(
                    text = contact.email,
                    fontSize = 12.sp,
                    color = Color.Gray
                )
            }
        }
    }
}

@Preview(showBackground = true)
@Composable
fun ContactListPreview() {
    MaterialTheme {
        ContactListScreen()
    }
}
```

---

## ↔️ LazyRow - Horizontal Lists

### Basic LazyRow

```kotlin
package com.example.lazylists

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyRow
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class LazyRowActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    LazyRowExamples()
                }
            }
        }
    }
}

@Composable
fun LazyRowExamples() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        Text(
            text = "Categories",
            fontSize = 20.sp,
            fontWeight = FontWeight.Bold,
            modifier = Modifier.padding(bottom = 12.dp)
        )
        
        CategoryRow()
        
        Spacer(modifier = Modifier.height(24.dp))
        
        Text(
            text = "Featured Items",
            fontSize = 20.sp,
            fontWeight = FontWeight.Bold,
            modifier = Modifier.padding(bottom = 12.dp)
        )
        
        FeaturedItemsRow()
    }
}

@Composable
fun CategoryRow() {
    val categories = listOf(
        "All", "Electronics", "Clothing", "Books", 
        "Home", "Sports", "Beauty", "Automotive"
    )
    
    LazyRow(
        horizontalArrangement = Arrangement.spacedBy(8.dp),
        contentPadding = PaddingValues(horizontal = 4.dp)
    ) {
        items(categories) { category ->
            CategoryChip(category)
        }
    }
}

@Composable
fun CategoryChip(category: String) {
    Box(
        modifier = Modifier
            .background(
                Color.Blue.copy(alpha = 0.1f),
                RoundedCornerShape(16.dp)
            )
            .padding(horizontal = 16.dp, vertical = 8.dp),
        contentAlignment = Alignment.Center
    ) {
        Text(
            text = category,
            color = Color.Blue,
            fontSize = 14.sp
        )
    }
}

@Composable
fun FeaturedItemsRow() {
    val colors = listOf(
        Color.Red, Color.Green, Color.Blue, Color.Yellow,
        Color.Magenta, Color.Cyan, Color.Gray, Color.Black
    )
    
    LazyRow(
        horizontalArrangement = Arrangement.spacedBy(12.dp),
        contentPadding = PaddingValues(horizontal = 4.dp)
    ) {
        items(colors.size) { index ->
            FeaturedItem(
                title = "Item ${index + 1}",
                color = colors[index]
            )
        }
    }
}

@Composable
fun FeaturedItem(title: String, color: Color) {
    Card(
        modifier = Modifier.width(120.dp),
        elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
    ) {
        Column {
            Box(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(80.dp)
                    .background(color.copy(alpha = 0.3f)),
                contentAlignment = Alignment.Center
            ) {
                Text(
                    text = "Image",
                    color = color
                )
            }
            
            Text(
                text = title,
                modifier = Modifier.padding(12.dp),
                fontSize = 14.sp,
                fontWeight = FontWeight.Medium
            )
        }
    }
}

@Preview(showBackground = true)
@Composable
fun LazyRowPreview() {
    MaterialTheme {
        LazyRowExamples()
    }
}
```

---

## 🔄 Mixed Content & Different Item Types

```kotlin
package com.example.lazylists

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

sealed class ListItem {
    data class Header(val title: String) : ListItem()
    data class Content(val text: String, val description: String) : ListItem()
    data class Ad(val content: String) : ListItem()
}

class MixedContentActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    MixedContentList()
                }
            }
        }
    }
}

@Composable
fun MixedContentList() {
    val mixedItems = listOf(
        ListItem.Header("Technology News"),
        ListItem.Content("AI Revolution", "Latest developments in artificial intelligence"),
        ListItem.Content("Quantum Computing", "Breakthrough in quantum processors"),
        ListItem.Ad("Special Offer: 50% off tech courses!"),
        ListItem.Header("Sports Updates"),
        ListItem.Content("World Cup Finals", "Exciting match results from yesterday"),
        ListItem.Content("Olympics Prep", "Athletes preparing for upcoming games"),
        ListItem.Ad("Get premium sports streaming now!"),
        ListItem.Header("Weather"),
        ListItem.Content("Sunny Week Ahead", "Perfect weather for outdoor activities")
    )
    
    LazyColumn(
        modifier = Modifier.fillMaxSize(),
        contentPadding = PaddingValues(16.dp),
        verticalArrangement = Arrangement.spacedBy(12.dp)
    ) {
        items(mixedItems) { item ->
            when (item) {
                is ListItem.Header -> HeaderItem(item.title)
                is ListItem.Content -> ContentItem(item.text, item.description)
                is ListItem.Ad -> AdItem(item.content)
            }
        }
    }
}

@Composable
fun HeaderItem(title: String) {
    Text(
        text = title,
        fontSize = 22.sp,
        fontWeight = FontWeight.Bold,
        color = Color.Blue,
        modifier = Modifier.padding(vertical = 8.dp)
    )
}

@Composable
fun ContentItem(title: String, description: String) {
    Card(
        modifier = Modifier.fillMaxWidth(),
        elevation = CardDefaults.cardElevation(defaultElevation = 2.dp)
    ) {
        Column(
            modifier = Modifier.padding(16.dp)
        ) {
            Text(
                text = title,
                fontSize = 16.sp,
                fontWeight = FontWeight.Bold
            )
            Spacer(modifier = Modifier.height(4.dp))
            Text(
                text = description,
                fontSize = 14.sp,
                color = Color.Gray
            )
        }
    }
}

@Composable
fun AdItem(content: String) {
    Box(
        modifier = Modifier
            .fillMaxWidth()
            .background(Color.Yellow.copy(alpha = 0.3f), RoundedCornerShape(8.dp))
            .padding(16.dp)
    ) {
        Text(
            text = "📢 $content",
            fontSize = 14.sp,
            fontWeight = FontWeight.Medium,
            color = Color.DarkGreen
        )
    }
}

@Preview(showBackground = true)
@Composable
fun MixedContentPreview() {
    MaterialTheme {
        MixedContentList()
    }
}
```

---

## ⚡ Performance Tips & Best Practices

### 1. Use Keys for Dynamic Lists
```kotlin
LazyColumn {
    items(
        items = dynamicList,
        key = { item -> item.id }  // ✅ Use unique keys
    ) { item ->
        ItemCard(item)
    }
}
```

### 2. Avoid Heavy Computations in Items
```kotlin
// ❌ Don't do heavy work inside items
LazyColumn {
    items(list) { item ->
        val processedData = heavyComputation(item)  // Bad!
        ItemView(processedData)
    }
}

// ✅ Process data beforehand
val processedList = remember { list.map { heavyComputation(it) } }
LazyColumn {
    items(processedList) { item ->
        ItemView(item)  // Good!
    }
}
```

### 3. Use contentPadding Instead of Wrapper Padding
```kotlin
// ✅ Better performance
LazyColumn(
    contentPadding = PaddingValues(16.dp)
) {
    items(list) { item -> ItemView(item) }
}
```

---

## 📖 Quick Comparison

| Feature | Column/Row | LazyColumn/LazyRow |
|---------|------------|-------------------|
| **Memory Usage** | Creates all items | Only visible items |
| **Performance** | Slow with many items | Fast with any amount |
| **Scrolling** | Can be janky | Always smooth |
| **Use Case** | Small, fixed lists | Large, dynamic lists |
| **Max Items** | ~50-100 items | Unlimited |

---

## ❓ Quick FAQs

### Q: When should I use LazyColumn vs Column?
**A:** Use LazyColumn when you have more than 20-30 items, or when the list size is dynamic.

### Q: Can I have headers in LazyColumn?
**A:** Yes! Use the `item { }` block for individual items like headers.

### Q: How do I handle clicking on list items?
**A:** Add `clickable` modifier or wrap in `Card` with `onClick`.

### Q: Can I mix LazyColumn and LazyRow?
**A:** Yes! You can put LazyRow inside LazyColumn items for horizontal scrolling sections.

### Q: How do I add spacing between items?
**A:** Use `verticalArrangement = Arrangement.spacedBy(8.dp)` in LazyColumn.

---

## 🎯 Practice Challenge (15 minutes)

Create a **Shopping App Screen** with:
1. Header with app name
2. Horizontal categories row (LazyRow)
3. Vertical product list (LazyColumn)
4. Different item types (products and ads)

```kotlin
// Your challenge - fill this in!
@Composable
fun ShoppingAppScreen() {
    // TODO: Implement the shopping app layout
}
```

---

## 📚 Next Topic Preview
**Coming up: UI State with `remember` & `mutableStateOf`** - Learn how to make your lists interactive with dynamic data!

## 🔧 Required Dependencies
```kotlin
dependencies {
    implementation "androidx.compose.foundation:foundation:1.5.4"
    implementation "androidx.compose.material3:material3:1.1.2"
}
```

**⏱️ Total Study Time: 60 minutes**
- Reading: 20 minutes
- Code examples: 25 minutes  
- Practice: 15 minutes
