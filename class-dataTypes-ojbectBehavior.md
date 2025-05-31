# 🧠 Lateral Learning Notes: Class, Type, Magic Methods, and Object Behavior

**Languages:** Python vs JavaScript/TypeScript vs C#

## 🔍 Topic | What

This document explores how classes, data types, constructors, operator overloading, and low-level object behavior differ across:

- Python (dynamic, introspective, powerful customization)
- JavaScript/TypeScript (prototype-based, limited low-level customization)
- C# (static, strongly typed, formal object system)

---

## 1. Classes and Object Construction

## 🐍 Python

- Constructor: `__init__`
- Dynamic attributes
- All types (even int) are classes
  
```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} speaks")

a = Animal("Dog")
a.speak()
```

## 🌐 JavaScript / TypeScript

- Class syntax is sugar over prototype
- No enforced access modifiers in JS (but present in TS)
- Lacks low-level hooks like Python
  
```typescript
class Animal {
    constructor(public name: string) {}

    speak() {
        console.log(`${this.name} speaks`);
    }
}

const a = new Animal("Dog");
a.speak();
```

## ⚙️ C #

- Strongly typed
- Access modifiers (public, private, etc.)
- Formal object model

```csharp
public class Animal {
    public string Name { get; set; }

    public Animal(string name) {
        Name = name;
    }

    public void Speak() {
        Console.WriteLine($"{Name} speaks");
    }
}

var a = new Animal("Dog");
a.Speak();
```

## 2. Data Types and Built-in Class Wrappers

## 🐍 Python

- All types are classes
- `int`, `str`, `list` are class instances
- Reflection: `type()`, `dir()`
  
```python
x = 42
print(type(x))  # <class 'int'>
```

## 🌐 JavaScript / TypeScript

- Primitive: `number`, `string`, etc.
- Wrapper objects: `new Number()`, `new String()`
  
```javascript
typeof 42           // "number"
typeof new Number(42)  // "object"
```

- TypeScript defines global types like `NumberConstructor`

```typescript
interface NumberConstructor {
    new (value?: any): Number;
    (value?: any): number;
}
```

## ⚙️ C #

- Value types (e.g. `int`) and reference types (e.g. strin`g)
- All types derive from `System.Object`

```csharp
int x = 42;
Console.WriteLine(x.GetType());  // System.Int32
```

## 3. Magic Methods / Operator Overloading

## 🐍 Python

- Custom behavior with `__dunder__` methods
- Hook into `+`, `[]`, `==`, iteration, etc.
  
```python
class MyNum:
    def __init__(self, value):
        self.value = value

    def __add__(self, other):
        return MyNum(self.value + other.value)

a = MyNum(5)
b = MyNum(7)
c = a + b  # calls __add__
```

## 🌐 JavaScript / TypeScript

- No built-in support for operator overloading
- Limited customization via `Symbol.toPrimitive` or `Proxy`
  
```javascript
class MyNum {
    constructor(value) {
        this.value = value;
    }

    add(other) {
        return new MyNum(this.value + other.value);
    }
}

let a = new MyNum(5);
let b = new MyNum(7);
let c = a.add(b);  // no operator overloading
```

## ⚙️ C #

- Supports operator overloading (`+`, `-`, `[]`, etc.)
- More formal and typed than Python

```csharp
public class MyNum {
    public int Value;

    public MyNum(int value) {
        Value = value;
    }

    public static MyNum operator +(MyNum a, MyNum b) {
        return new MyNum(a.Value + b.Value);
    }
}

var result = new MyNum(1) + new MyNum(2);  // operator + overloaded
```

## 4. Interfaces and Prototypes

## 🐍 Python

- No formal interfa`ce keyword
- Duck typing: "if it walks like a duck..."
- Use ABCs (`abc.ABC`) for optional structural enforcement

## 🌐 JavaScript / TypeScript

- inter`face = TypeScript-only (compile-time)
- JS uses prototype delegation instead of interfaces
  
```typescript
interface Speakable {
    speak(): void;
}

class Dog implements Speakable {
    speak() {
        console.log("Woof!");
    }
}
```

## ⚙️ C #

- Interfaces are first-class citizens
- Enforced at compile time and respected by the CLR

```csharp
public interface ISpeakable {
    void Speak();
}

public class Dog : ISpeakable {
    public void Speak() {
        Console.WriteLine("Woof!");
    }
}
```

## 🧠 Takeaway | Why

- Python is dynamic and reflective: all types are classes, and magic methods give powerful customization.
- JavaScript is prototype-based: limited low-level control; TypeScript adds compile-time typing but not runtime behavior.
- C# is strict and static: classes, interfaces, and operators are strongly typed, formal, and compile-time enforced.
- Magic method behavior (`__add__`, `__getitem__`, etc.) is unique to Python.
- TypeScript interfaces model JS runtime behavior but are erased at runtime.
- JS has no true operator overloading; C# and Python do.
