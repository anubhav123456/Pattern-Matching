**Pattern Matching for `instanceof`** is a modern Java feature that makes type checks **cleaner, safer, and less verbose**.

---

## ❌ Old way (before Java 16)

```java

Object obj = "Hello";

if (obj instanceof String) 
{
    String s = (String) obj;
    System.out.println(s.length());
}
```

Problems:

* Repeated variable
* Explicit casting
* Easy to make mistakes

---

## ✅ New way: Pattern Matching for `instanceof` (Java 16+)

```java
Object obj = "Hello";

if (obj instanceof String s) 
{
    System.out.println(s.length());
}
```

What happens:

* Java **checks the type**
* **Casts automatically**
* **Creates a variable (`s`)** safely

---

## 🔹 How it works

```java
if (obj instanceof Type variableName) {
    // variableName is already casted
}
```

The variable is:

* **Final implicitly**
* **Available only inside the true branch**

---

## 🔸 Example with custom class

```java
class User 
{
    String name;
}

Object obj = new User();

if (obj instanceof User u) 
{
    System.out.println(u.name);
}
```

---

## 🔹 With logical conditions

```java
Object obj = "Hello";

if (obj instanceof String s && s.length() > 5) 
{
    System.out.println("Long string");
}
```

👉 `s` is available **after** the `instanceof` check passes.

```java
Object obj = 5;

if (obj instanceof Integer i && i < 10) 
{
    System.out.println(i);
}

👉 `i` is available **after** the `instanceof` check passes.
```

---

## 🔸 NOT allowed ❌

```java
Object obj = 5;

if (obj instanceof String s || s.length() > 5) 
{
    // Compile-time error
}
```

Reason: `s` may not be initialized.

```java
 Object obj = 5;
 
if(obj instanceof Integer i || obj instanceof String s)
{
    System.out.println(i);
}
```
In pattern matching, variables introduced in different OR branches are not definitely assigned together, so they cannot be used inside the common block.

---

## ✅ Correct way (separate blocks)
```java
 Object obj = 5;
 
if(obj instanceof Integer i)
{
    System.out.println(i);
}
else if(obj instanceof String s)
{
    System.out.println(s);
} 
```

---
## 🔹 Pattern matching with interfaces

* Without Pattern Maching
```java
interface Vehicle
{
    void drive();
}

class TwoWheeler implements Vehicle
{

    @Override
    public void drive()
    {
        System.out.println("Two wheeler drive implementation");
    }
}

class FourWheeler implements Vehicle
{

    @Override
    public void drive()
    {
        System.out.println("Four wheeler drive implementation");
    }
}

public class Main
{
    public static void main(String[] args)
    {
        Object obj = new TwoWheeler();
        if(obj instanceof TwoWheeler)
        {
            TwoWheeler obj1 = (TwoWheeler) obj;
            obj1.drive();
        }
        else if(obj instanceof FourWheeler)
        {
            FourWheeler obj2 = (FourWheeler) obj;
            obj2.drive();
        }
    }
}
```

```
Two wheeler drive implementation
```

* With Pattern Maching

```java
public class Main
{
    public static void main(String[] args)
    {
        Object obj = new TwoWheeler();
        
        if(obj instanceof Vehicle v)
        {
            v.drive();
        }

    }
}
```

```
Two wheeler drive implementation
```
---

## 🔹 Scope rules (important for interviews)

```java
if (!(obj instanceof String s)) {
    return;
}
// s is accessible here
System.out.println(s.length());
```

✔️ Java understands control flow and keeps `s` in scope.

---

## 🔥 Real-world use (you’ll love this in backend)

```java
public void process(Object obj) 
{
    if (obj instanceof Order order) 
    {
        order.process();
    } 
    else if (obj instanceof Customer customer) 
    {
        customer.notifyUser();
    }
}
```

Much cleaner than casting everywhere.

---

## 🧠 Interview one-liner

> *Pattern Matching for `instanceof` removes explicit casting by combining type check and variable binding in a single expression.*

---

## ⚙️ Java version support

| Java Version | Status  |
| ------------ | ------- |
| Java 14–15   | Preview |
| **Java 16+** | Stable  |

---

