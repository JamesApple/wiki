# Building java with maven

Maven is simplest when used with this [standard repository layout](http://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)

`./pom.xml`

```xml
<project>
   <modelVersion>4.0.0</modelVersion>
   <groupId>jamesapple.example</groupId>
   <artifactId>CompilingAndRunningWithoutIDE</artifactId>
   <version>1</version>
   <build>
      <defaultGoal>compile</defaultGoal>
      <sourceDirectory>src</sourceDirectory>
      <outputDirectory>classes</outputDirectory>
      <finalName>${project.artifactId}-${project.version}</finalName>
   </build>
</project>
```

```sh
mvn # will compile as this is the default goal
mvn exec:java -Dexec.mainClass=jamesapple.example.Main
```
