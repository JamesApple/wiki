# Building with `javac` and running `java`
#java #language 

`src/Child.java`
```java
package jamesapple.example;

public class Child extends Parent
{
   @Override
   public String toString()
   {
      return "I'm the Child.";
   }
}
```

`src/Parent.java`
```java
package jamesapple.example;

public class Parent
{
   @Override
   public String toString()
   {
      return "I'm the Parent.";
   }
}
```

`src/Main.java`
```java
package jamesapple.example;

import static java.lang.System.out;

public class Main {
   private final Parent parent = new Parent();
   private final Child child = new Child();

   public static void main(final String[] arguments) {
      final Main instance = new Main();
      out.println(instance.parent);
      out.println(instance.child);
   }
}
```

```sh
mkdir classes
javac -sourcepath src -d classes ./src/*
java -classpath classes jamesapple.example.Main
```
