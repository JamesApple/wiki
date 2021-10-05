# Building with gradle

http://marxsoftware.blogspot.com/2013/12/simple-gradle-java-plugin-customization.html

`build.gradle`

```groovy
apply plugin: 'java'
apply plugin: 'application'
// Redefine where Gradle should expect Java source files (*.java)
sourceSets {
    main {
        java {
            srcDirs 'src'
        }
    }
}
// Redefine where .class files are written
sourceSets.main.output.classesDir = file("classes")
// Specify main class to be executed
mainClassName = "jamesapple.example.Main"
defaultTasks 'compileJava', 'run'
```

`gradle`
