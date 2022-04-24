#java #language 
Ant is the simplest of the 3 build tools for java

`build.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="ANameISuppose" default="run" basedir=".">
   <description>Build This THING</description>

   <!-- Ant target for compile is a list of actions(?) -->
   <target name="compile"
           description="Compile the Java code.">

      <!-- javac in xml form -->
      <javac srcdir="src"
             destdir="classes"
             debug="true"
      includeantruntime="false" />

   </target>

   <!-- run target. Always runs compile first -->
   <target name="run" depends="compile"
           description="Run the Java application.">

      <!-- Run this class -->
      <java classname="jamesapple.example.Main" fork="true">
         <!-- List of classpaths? -->
         <classpath>
           <pathelement path="classes"/>
         </classpath>
      </java>

   </target>

</project>
```

Install ant
