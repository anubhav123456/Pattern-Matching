**Pattern Matching for `instanceof`** is a modern Java feature that makes type checks **cleaner, safer, and less verbose**.

---

## ❌ Old way (before Java 16)

```java
if (obj instanceof String) {
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
if (obj instanceof String s) {
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
class User {
    String name;
}

Object obj = new User();

if (obj instanceof User u) {
    System.out.println(u.name);
}
```

---

## 🔹 With logical conditions

```java
if (obj instanceof String s && s.length() > 5) {
    System.out.println("Long string");
}
```

👉 `s` is available **after** the `instanceof` check passes.

---

## 🔸 NOT allowed ❌

```java
if (obj instanceof String s || s.length() > 5) {
    // Compile-time error
}
```

Reason: `s` may not be initialized.

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
public void process(Object obj) {
    if (obj instanceof Order order) {
        order.process();
    } else if (obj instanceof Customer customer) {
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

