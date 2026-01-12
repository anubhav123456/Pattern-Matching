
---

## 1️⃣ What is Pattern Matching for `switch`?

Pattern matching lets you:

* **Match type + condition together**
* **Eliminate `instanceof` + casting**
* Write **cleaner, safer, and more expressive code**

---

## 2️⃣ Old Way (Before Java 21)

```java
static String process(Object obj) {
    if (obj instanceof Integer) {
        Integer i = (Integer) obj;
        return "Integer: " + i;
    } else if (obj instanceof String) {
        String s = (String) obj;
        return "String: " + s;
    }
    return "Unknown";
}
```

❌ Problems:

* Verbose
* Unsafe casting
* Hard to extend

---

## 3️⃣ Java 21 Pattern Matching with `switch`

```java
static String process(Object obj) {
    return switch (obj) {
        case Integer i -> "Integer: " + i;
        case String s  -> "String: " + s;
        case null      -> "Null value";
        default        -> "Unknown";
    };
}
```

✅ Benefits:

* No casting
* Handles `null`
* Exhaustive & safer

---

## 4️⃣ Using **Guards** (`when` condition)

```java
static String classifyNumber(Object obj) {
    return switch (obj) {
        case Integer i when i > 0 -> "Positive Integer";
        case Integer i when i < 0 -> "Negative Integer";
        case Integer i           -> "Zero";
        default                  -> "Not an Integer";
    };
}
```

➡️ **Interview keyword**: *Guarded Patterns*

---

## 5️⃣ Pattern Matching with Records (Very Important)

```java
record User(String name, int age) {}

static String checkUser(Object obj) {
    return switch (obj) {
        case User(String name, int age) when age >= 18 ->
            name + " is an adult";
        case User(String name, int age) ->
            name + " is a minor";
        default ->
            "Not a user";
    };
}
```

🔥 Very common **Java 21 interview question**

---

## 6️⃣ Sealed Classes + Pattern Matching (Enterprise Use)

```java
sealed interface Shape permits Circle, Rectangle {}

record Circle(double radius) implements Shape {}
record Rectangle(double length, double width) implements Shape {}

static double area(Shape shape) {
    return switch (shape) {
        case Circle c    -> Math.PI * c.radius() * c.radius();
        case Rectangle r -> r.length() * r.width();
    };
}
```

✅ No `default` needed
➡️ **Exhaustiveness guaranteed by compiler**

---

## 7️⃣ `switch` Expression vs Statement

### Expression (returns value)

```java
int result = switch (day) {
    case 1, 2, 3 -> 10;
    case 4, 5    -> 20;
    default      -> 0;
};
```

### Statement (no return)

```java
switch (day) {
    case 1 -> System.out.println("Monday");
}
```

---

## 8️⃣ Handling `null` in Java 21

```java
switch (obj) {
    case null -> System.out.println("Null");
    case String s -> System.out.println(s);
    default -> System.out.println("Other");
}
```

❗ Without `case null`, `NullPointerException` occurs.

---

## 9️⃣ Interview Questions (Java 21 – MUST KNOW)

### ❓ Why pattern matching in switch?

**Answer:**

* Reduces boilerplate
* Improves readability
* Compile-time exhaustiveness checks
* Eliminates unsafe casts

---

### ❓ Difference between `instanceof` and switch pattern matching?

| Feature          | instanceof | switch   |
| ---------------- | ---------- | -------- |
| Multiple types   | ❌          | ✅        |
| Exhaustive check | ❌          | ✅        |
| Readability      | Medium     | High     |
| Null handling    | Manual     | Built-in |

---

### ❓ Can switch work with Object?

✅ Yes (Java 21)

---

### ❓ What happens if default is missing?

* Allowed **only** if all possible cases are covered (sealed classes)

---

## 🔥 One-Line Summary (Interview Gold)

> **Java 21 pattern matching for switch allows type-safe, expressive, and exhaustive control flow by combining type checks, casting, and conditions in a single construct.**

---
