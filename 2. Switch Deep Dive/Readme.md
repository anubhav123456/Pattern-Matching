# Java 14 Switch
---

## 1️⃣ Traditional (Old) Switch Statement

### Enum Example

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

### Classic Switch Syntax

```java
Day day = Day.MONDAY;

switch (day) {
    case MONDAY:
        System.out.println(6);
        break;
    case TUESDAY:
        System.out.println(7);
        break;
    case WEDNESDAY:
        System.out.println(9);
        break;
    case THURSDAY:
        System.out.println(8);
        break;
    case FRIDAY:
        System.out.println(6);
        break;
    case SATURDAY:
        System.out.println(8);
        break;
    case SUNDAY:
        System.out.println(6);
        break;
}
```

---

## 2️⃣ Problem #1: Verbose Case Stacking

### Old Way (Multiple Lines)

```java
public class Main 
{
    // Defining the Enum used in the switch
    enum Days {
        MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
    }

    public static void main(String[] args) 
    {
        Days day = Days.FRIDAY;

        // Switch statement as shown in the image
        switch (day) 
        {
            case MONDAY:
            case FRIDAY:
            case SUNDAY:
                System.out.println(6);
                break;
                
            case TUESDAY:
                System.out.println(7);
                break;
                
            case THURSDAY:
            case SATURDAY:
                System.out.println(8);
                break;
                
            case WEDNESDAY:
                System.out.println(9);
                break;
        }
    }
}
```

### Java 14 Solution: Comma-Separated Labels

```java
switch (day) 
{
    case MONDAY, FRIDAY, SUNDAY:
        System.out.println(6);
        break;
    case TUESDAY:
        System.out.println(7);
        break;
    case THURSDAY, SATURDAY:
        System.out.println(8);
        break;
    case WEDNESDAY:
        System.out.println(9);
        break;
}
```

✔ Cleaner and less verbose

---

## 3️⃣ Problem #2: Fall-Through by Default

### Bug Example (Missing `break`)

```java
//Switch statement
Days day = Days.FRIDAY;

switch (day) 
{
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        System.out.println(6);
    case TUESDAY:
        System.out.println(7);
        break;
    case THURSDAY:
    case SATURDAY:
        System.out.println(8);
        break;
    case WEDNESDAY:
        System.out.println(9);
        break;
}
```

### Output

```
6
7
```

❌ Bug caused due to fall-through

---

## 4️⃣ Java 14 Feature: Arrow (`->`) Switch Labels

### No Fall-Through, No Break Needed

```java
switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY -> System.out.println(7);
    case THURSDAY, SATURDAY -> System.out.println(8);
    case WEDNESDAY -> System.out.println(9);
}
```

✔ Each case executes independently
✔ No accidental fall-through

---

## 5️⃣ Problem #3: Switch Could Not Return a Value (Old Style)

### Old Workaround Using Variable

```java
Days day = Days.FRIDAY;

int count = 0;
switch (day) 
{
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        count = 6;
        break;
    case TUESDAY:
        count = 7;
        break;
    case THURSDAY:
    case SATURDAY:
        count = 8;
        break;
    case WEDNESDAY:
        count = 9;
        break;
}

System.out.println(count);
```

❌ Extra variable + mutation

---

## 6️⃣ Java 14 Feature: Switch as an Expression

### Returning Value Directly

```java
int count = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY -> 7;
    case THURSDAY, SATURDAY -> 8;
    case WEDNESDAY -> 9;
};
```

✔ Cleaner
✔ Immutable
✔ Expression-based

⚠ **Semicolon is mandatory** after switch expression

---

## 7️⃣ Multiple Statements: `yield` Keyword

### When Case Has Multiple Statements

```java
int count = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> {
        if (day == Day.SUNDAY) {
            throw new RuntimeException("Sunday is a holiday");
        }
        yield 6;
    }
    case TUESDAY -> 7;
    case THURSDAY, SATURDAY -> 8;
    case WEDNESDAY -> 9;
};
```

### Key Points

* `yield` returns value from switch expression
* Required when using a block `{}`

---

## 8️⃣ Rule: Exhaustiveness Check (Very Important)

### Compiler Forces All Cases (Only for Expressions)

❌ Invalid Code

```java
int count = switch (day) {
    case MONDAY -> 6;
    case TUESDAY -> 7;
};
```

### Compiler Error

```
Switch expression does not cover all possible input values
```

### Fix #1: Cover All Enum Values

```java
case WEDNESDAY -> 9;
case THURSDAY -> 8;
case FRIDAY -> 6;
case SATURDAY -> 8;
case SUNDAY -> 6;
```

### Fix #2: Use `default`

```java
default -> throw new IllegalStateException("Unexpected value");
```

---

## 9️⃣ Statement vs Expression (Easy Rule)

| Type           | Meaning                              |
| -------------- | ------------------------------------ |
| **Statement**  | Just executes code (no return value) |
| **Expression** | Must return a value                  |

### Statement Example

```java
switch (day) {
    case MONDAY -> System.out.println("Weekday");
}
```

### Expression Example

```java
String type = switch (day) {
    case SATURDAY, SUNDAY -> "Weekend";
    default -> "Weekday";
};
```

---

## 🔟 Problem #4: Shared Scope in Old Switch

### Old Switch Scope Issue

```java
switch (day) {
    case MONDAY:
        String val = "Monday";
        break;
    case TUESDAY:
        String val = "Tuesday"; // ❌ Compile error
        break;
}
```

❌ Same scope for all cases

---

## 1️⃣1️⃣ Java 14 Fix: Independent Scope with Arrow

```java
String result = switch (day) {
    case MONDAY -> {
        String val = "Monday";
        yield val;
    }
    case TUESDAY -> {
        String val = "Tuesday";
        yield val;
    }
    default -> "Other";
};
```

✔ Each case has its own scope

---

## 1️⃣2️⃣ Mixing Old (`:`) and New Features

### Colon (`:`) Still Means Fall-Through

```java
String type = switch (day) {
    case SATURDAY, SUNDAY:
        yield "Weekend";
    case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY:
        yield "Weekday";
};
```

### Important Rules

| Syntax           | Behavior              |
| ---------------- | --------------------- |
| `:`              | Fall-through possible |
| `->`             | No fall-through       |
| `:` + expression | Use `yield`           |
| `:` + statements | Use `break`           |

---

## 1️⃣3️⃣ Final Summary

* ✔ Java 14 switch is **cleaner, safer, and expression-based**
* ✔ `->` removes fall-through bugs
* ✔ `yield` enables value return from blocks
* ✔ Compiler enforces exhaustiveness
* ✔ Each arrow case has independent scope

---

📌 **Tip:**

* > If you see `:` → think *fall-through*
* > If you see `->` → think *safe & isolated*

---
