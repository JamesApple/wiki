#java #language 
# Classes
```java
// Inherit
public class MyClass extends AnotherClass {}

// Implement an interface
public class MyClass implements MyInterface {}

// Initializer
// MyClass newMyClass = new MyClass(anotherClass)
public class MyClass {
  public AnotherClass myVar;

  public MyClass(AnotherClass myVar) {
    this.myVar = myVar;
  }

  // Override initializer. If passed these types will use this initializer
  public MyClass(DifferentClass myMar, AnotherClass myOtherVar) {
    // do something
  }

  // Override Ancestor. In this case from `Object`
  @Override
  public string toString() {}
}
```

# Methods

## Visibility:

1. public -> Visible anywhere
1. protected -> Visible to members of the same package and subclasses
1. private -> Visible only to the class itself


protected 
```java
public ReturnType methodName(ArgType argName) {
  return arName.toReturnType()
}
```

# Variables
```java
public class MyClass {
  private String myInstanceVar
  public static String myClassString
  public static final String myConstant = "MY_CONSTANT"
}

```

# Types

```java
// 32bit 2,147,483,647 +/-
int number = 2
// 64bit 9,223,372,036,854,775,807 +/-
long number = 2

float
double

// 128 -> -127
byte number = 2

// 32,767 -> -32,768
short number = 2

boolean a = true

// Characters use -> ''
char a = 'a'

String s = "asd"

```

# Boxing / Autoboxing

Primitives like `int` may need to be boxed to actual objects to be passed into
a class such as `ArrayList`. 
```java
Integer boxed = new Integer(1)
Integer boxed = 1
```

# Casting

```java

// Cast int to long
long number = (long)intNumber
Person person = (Person)existingSubclass
if (person instanceof Human) { ... }
```

# Control-flow
```java
if (condition) {  }
else {}

switch(number) {
case 10:
...;
default:
...;
}

for (int i = 0; i > 0; i++) { i }

for (int num : numArray) { item }

while (boolFunc()) {  }

do {  } while(boolFunc())


```

# Collections

```java
// type[] name = new type[size]
int[] myArray = new int[10]
int[] myArray = { 0, 1, 2, 3, 4, 5 }
myArray.length
// Multidimensional
int[][] = new int[10][10]

// Integer array list starting with 5
ArrayList<Integer> myList = new ArrayList<>(5);
myList.add(2)
myList.set(1, 2)
```

# Exceptions
```java
try { 1 / 0 } catch(ArithmeticException e) { }
```
