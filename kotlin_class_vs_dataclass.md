# Kotlin: Class vs Data Class

## Regular Class

A standard class that requires manual implementation of common methods.

```kotlin
class Person(val name: String, val age: Int) {
    
    // Must manually implement these methods
    override fun toString(): String {
        return "Person(name='$name', age=$age)"
    }
    
    override fun equals(other: Any?): Boolean {
        if (this === other) return true
        if (other !is Person) return false
        return name == other.name && age == other.age
    }
    
    override fun hashCode(): Int {
        return name.hashCode() * 31 + age.hashCode()
    }
    
    // No copy method available
}
```

## Data Class

Automatically generates common methods for data containers.

```kotlin
data class Person(val name: String, val age: Int)
// That's it! Everything else is auto-generated
```

## Key Differences

| Feature | Regular Class | Data Class |
|---------|---------------|------------|
| `toString()` | Manual implementation | Auto-generated |
| `equals()` & `hashCode()` | Manual implementation | Auto-generated |
| `copy()` method | Not available | Auto-generated |
| `componentN()` functions | Not available | Auto-generated |
| Primary purpose | General-purpose | Data containers |

## Usage Examples

```kotlin
fun main() {
    // Data class usage
    val person1 = Person("Alice", 25)
    val person2 = Person("Alice", 25)
    val person3 = person1.copy(age = 26) // Easy copying with modification
    
    println(person1) // Person(name=Alice, age=25) - auto toString()
    println(person1 == person2) // true - auto equals()
    
    // Destructuring (componentN functions)
    val (name, age) = person1
    println("Name: $name, Age: $age")
}
```

## When to Use

**Use Data Class when:**
- Storing data/state
- Need `equals()`, `hashCode()`, `toString()`
- Want easy copying with modifications
- Need destructuring support

**Use Regular Class when:**
- Complex business logic
- Custom behavior for equality/toString
- Inheritance required (data classes are final)
- Need more control over class structure