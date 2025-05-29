# 🧠 Lateral Learning Notes: OOP & `self` vs `this`  

**Languages:** Python vs JavaScript/TypeScript vs C#

## 🔍 Topic | What

How object-oriented programming works in Python versus JavaScript/TypeScript and C#.

---

## 🐍 Python : methods are just functions

```python
# Tiggering question:
class Cat:
    def speak(self):
        return "喵"
    
garfield = Cat()
print(garfield.speak())
# print(Cat.speak()) # TypeError: Cat.speak() missing 1 required positional argument: 'self'
```

- In Python, **instance methods** are just regular functions stored in a class. The `self` parameter isn’t magical — it's the first argument that Python passes **automatically** when you call the method on an instance.
  
```python
# You manually fill in the 'self':
print(Cat.speak(garfield))  
```

-So what if passing a `self` from a different class?
  
```python

class Dog:
    def speak(self):
        return "汪"
snoopy = Dog()
print(Cat.speak(snoopy))
```

- 🚨 Fun but **Danger!!!**

## 🌐 JavaScript / TypeScript: methods are properties on the class prototype and binding is more dynamic
  
```javascript
// Example:
class Cat {
    speak() {
        return "喵";
    }
}

const garfield = new Cat();
console.log(garfield.speak());
console.log(Cat.speak()); // TypeError: Cat.speak is not a function
console.log(Cat.speak(garfield)); // TypeError: Cat.speak is not a function
```

- Methods are properties on the class prototype.
- The `this` binding is resolved at call time, not via a required first argument like `self`.
- `Cat.speak` simply doesn’t exist unless it was defined as a **static method**.

```javascript
class Cat {
    static speak() {
        return "喵"
    }
}

console.log(Cat.speak())

const garfield = new Cat();
console.log(garfield.speak()); // TypeError: garfield.speak is not a function
```

## ⚙️ C#: More Rigid and Explicit

```csharp
// Example:
class Cat
{
    public string Speak()
    {
        return "喵"
    }
}

var garfield = new Cat();
Console.WriteLine(garfield.Speak());

Console.WriteLine(Cat.Speak()); // ❌ Compile-time error
// error CS0120: An object reference is required for the non-static field, method, or property 'Cat.Speak()'
```

- In C#, instance methods must be called on an instance. The compiler enforces this — you can’t even try to call it without an object. It’s a **strongly typed, compiled** language, so misuse like this is caught early.

```csharp
class Cat
{
    public static string Speak()
    {
        return "喵";
    }
}


Console.WriteLine(Cat.Speak()); 

var garfield = new Cat();
Console.WriteLine(garfield.Speak());
// error CS0176: Member 'Cat.Speak()' cannot be accessed with an instance reference; qualify it with a type name instead
```

## 🧠 Takeaway | Why

- In Python, methods are just functions. The instance (`self`) must be passed either implicitly (via `garfield.speak()`) or explicitly (via Cat.speak(garfield))

- In JS/TS, methods don't take an implicit `self` — instead, this is bound dynamically based on how the function is called. This allows for more flexibility but also more confusion.
  
- C# is strict about method access — class methods must be marked static, and instance methods must be called on an object.
