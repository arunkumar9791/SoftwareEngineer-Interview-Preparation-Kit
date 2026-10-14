# Anonymous Class

Anonymous class is a class without a name that you create and use immediately.

### Normal class

```java
class Animal {
    void sound()
    {
        System.out.println("Animal Sound");
    }
}
```

### Anonymous class

```java
public class Main {
    public static void main(String[] args)
    {
        Animal a = new Animal() {
            @Override
            void sound() {
                System.out.println("Bark");
            }
        };
        a.sound();
    }
}
```

- `Animal` → Existing class
- `a` → Object reference
- `new Animal() { ... }` → defines a new, unnamed class that overrides `sound()` method

### Why anonymous class

When we need a special version of a class only once.

---

| Anonymous class | Normal class |
|---|---|
| Has no name | Has a name |
| Defined where the object is created | Declared separately |
| Usually used for specific one-off implementation | Can be reused easily |

An anonymous class can be created by

- Extending a normal class
- Implementing an interface such as Runnable.

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Inside Run()");
    }
};
r.run();
```
