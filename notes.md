## 1.1 Overview

Kotlin is a language used to write applications to be run on the Java Virtual Machine (JVM); it is regarded as a "better Java than Java".

> JVM docs: https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-1.html#jvms-1.

> The Java Virtual Machine is the cornerstone of the Java platform. It is the component of the technology responsible for its hardware- and operating system-independence, the small size of its compiled code, and its ability to protect users from malicious programs.

> The Java Virtual Machine is an abstract computing machine. Like a real computing machine, it has an instruction set and manipulates various memory areas at run time...The Java Virtual Machine knows nothing of the Java programming language, only of a particular binary format, the class file format. A class file contains Java Virtual Machine instructions (or bytecodes)

## 2.1 Course introduction

Evidently, programming is hard in Java because of checking for null values.

There is "way less ceremony" than Java. Fewer lines of code to achieve the same result.

## 2.2 Installing Kotlin

Assuming you've installed it, run `kotlinc` from the command line to open the Kotlin compiler. This opens a Kotlin REPL - a "Read Evaluate Print Loop".

> A read–eval–print loop (REPL), also termed an interactive toplevel or language shell, is a simple interactive computer programming environment that takes single user inputs, executes them, and returns the result to the user; a program written in a REPL environment is executed piecewise

A browser console is a REPL for JavaScript.

`:quit` to quit the Kotline REPL.

Now, outside of that word, Kotlin files have a `.kt` extension. You would generate a `.class` file (similar to compiling Java, since this is for the JVM) by running `kotlinc [kotlinfile].kt`.

Let's compare the Hello World programs in Java vs Kotlin:

```java
/* HelloWorld.java */
public class HelloWorld {
  public static void main(String[] args) {
    System.out.println("Hello world!");
  }
}
```
```kotlin
// HelloWorld.kt
fun main(args: Array<String>) {
  println("Hello, world")
}

// Compiles to a file named "HelloWorldKt.class" (note the Kt in the name)
```
The above Kotlin example compiles to a valid `.class` file. How? We didn't declare a class. The compiler constructs a class on our behalf and marks that function as a static function within the class.

Let's note some other key differences. In the Kotlin example:

- The variable name comes first, THEN the type
- An array is not implied with `Type[]`, instead it is explicit: `Array<Type>`
- There is no class
- There is no return type (because it defaults to a `unit` return type, this is like `void` in Java and how JS implicitly returns `undefined`)
- No semicolon at the end of the `println`

Something to look into. This doesn't work:

```
kotlinc HelloWorld.kt
java HelloWorldKt
```

But per the tutorial you can do this.
```
kotlinc HelloWorld.kt -include-runtime -d HelloWorld.jar
java -jar HelloWorld.jar
```

Here's the answer.

> When you write an application in Java, you get to rely on all of the standard class libraries. The java. classes (e.g. java.lang.*, java.util.* ...) are included with every JRE, so you don't need to package them yourself.

> Kotlin includes its own standard class library (the Kotlin runtime), separate to the Java class library. To distribute a jar file that can be run by anyone with a plain old JRE, you need to bundle the Kotlin runtime as well.
