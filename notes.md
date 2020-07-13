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

# 2.3 What is Kotlin

- It's a JVM language, meaning it compiles to Java bytecode.
- It's object oriented
- It's also functional, supports higher order functions. Functions are first-class citizens.

# 2.4 Basic Coding in Kotlin

`var` is for reassignable variables; `val` is for immutable ones.

```kotlin
// com/rsk/Person.kt
package com.rsk

// note the constuctor is the argument to the class.
// Also note: you use `var` or `val` in constructors, but not regular funcs.
class Person(var name: String) {
    fun displayName() {
        println("The person's name is $name")
    }

    fun callExternalFuncWithName(externalFunc: (name:String) -> Unit) {
        externalFunc(Name);
    }
}

// main.kt
import com.rsk.Person

fun main(args: Array<String>) {
    println("Hello, world");

    val kevin = Person("Kevin");
    val kevinsFavoriteNumber = 3;

    kevin.displayName()

    kevin.callExternalFuncWithName(::makeFunOf)
}

fun makeFunOf(var name: String){
    println("$name IS A DOODIE HEAD")
}
```
In Java you would have had to declare both the class and its method `displayName` as `public`. But in Kotlin that's the default behavior.

Note the demonstration of Kotlin's ability to accept functions as an argument with its function `callExternalFuncWithName`.

Also note that Kotlin supports the "${variable}" syntax, but the IDE recommends the more concise "$variable" syntax IF the variable is not a property accessor. But if you want to interpolate `obj.value` (a "compound variable"), then you must use "${obj.value}"

Note the way you have to reference functions outside of their definition - with `::`. Evidently this is similar to Java.

Note also that we were able to declare variables without declaring their type. It's inferred from what's going on on the right hand side of that assignment.

Note also there's no `new` keyword when utilizing the `Person` constructor.

## 3.1 Getting started with Kotlin - intro

Again, why it may be better than Java:

- No `new` keyword
- No getters/setters
- No need to anticipate the type of exception that may be thrown


## 3.2 Using Kotlin Without Creating Any Class Definitions

Uses a tool called `fernflower` to decompiled the compiled `MainKt.class` file to see what the compiler does to make the Kotlin source compatible with the JVM. As we said before, if the source does not contain a class it will wrap that in a `class`, and functions would be `static` members of that class.

Here's that output without the Metadata decorator:
```java
import kotlin.Metadata;
import kotlin.jvm.internal.Intrinsics;
import org.jetbrains.annotations.NotNull;

public final class MainKt {
   public static final void main(@NotNull String[] args) {
      Intrinsics.checkParameterIsNotNull(args, "args");
      String var1 = "Hello, world";
      System.out.println(var1);
   }
}
```

If you need, you can go back here to see how he configured IntelliJ to export a .jar file with the Kotlin runtime bundled in the "artifacts" directory.

## 3.3 Kotlin's support for immutability

`var` means variable (reassignable); `val` means value (not reassignable).

In Java, files and classes need to map 1:1, and the class name needs to be the same as the file name.

Not in Kotlin. You can declare a `class` in the same file as a stand-alone `fun`.

Kotlin supports variable assignment types, but doesn't require them:

```kotlin
// either of these work:
var firstQ = Question()
var secondQ:Question = Question()
```

He mentions a "JavaBean". What's that?

https://stackoverflow.com/questions/3295496/what-is-a-javabean-exactly
> A JavaBean is just a standard:

> - All properties private (use getters/setters)
> - A public no-argument constructor
> - Implements Serializable.

So when you do create a class, it makes it sort of like a JavaBean, except without that serializable part:

```kotlin
class Person(var name: String) {
}
```
...compiles to:
```java
import kotlin.Metadata;
import kotlin.jvm.internal.Intrinsics;
import org.jetbrains.annotations.NotNull;

public final class Person {
   @NotNull
   private String name;

   @NotNull
   public final String getName() {
      return this.name;
   }

   public final void setName(@NotNull String var1) {
      Intrinsics.checkParameterIsNotNull(var1, "<set-?>");
      this.name = var1;
   }

   public Person(@NotNull String name) {
      Intrinsics.checkParameterIsNotNull(name, "name");
      super();
      this.name = name;
   }
}
```
Note the most important thing here - when you declare a variable within a Kotlin class, or in the constructor, the compiler will convert it to a private variable with a public-facing getter and setter.

# 3.4 String Templates in Kotlin

(covered earlier)

# 3.5 Using 'if' as an expression

This code works:
```kotlin
if (q.answer == q.correct) {
  println("You're right")
} else {
  println("You're wrong")
}
```
In Java, this wouldn't work because `==` is strictly for reference equality, and those are two different properties being compared. You would have to use `q.answer.equals(q.correct)`

When variables are declared they have to be initialized. The only exception is if the variable is an `abstract` one, but too early to know how to use that yet.

In Kotlin `if` is an *expression*. So you can do this:

```kotlin
val message = if (q.answer == q.correct) {
  println("You're right")
} else {
  println("You're wrong")
}
```

## 3.6 How Kotlin Improves the Handling of Null Values

In Kotlin, we have to explicitly tell the compiler that something can take on a null value, rather than anything being nullable and always guarding against it.

We do this with the `?` appended to the type to signify that the value can be null.

```kotlin
class Person(var name: String) {
  var veteranStatus: String? = null
  var firstChild:Person? = null

  // The below will throw an error because `firstChild` is not guaranteed to be non-null! Thanks Kotlin!
  val firstChildName = firstChild.name

  // The proper way to do it. If `firstChild` does not exist, the returned value is `null`.
  val firstChildName = firstChild?.name
}
```

## 3.7 The 'When' Statement in Kotlin

There is no `switch` statement in Kotlin. But there is `when`:

```kotlin
fun choose(flavor:String?) {
  when (flavor) {
    "lemon" -> println("Nice, sour is good")
    "pig kidney" -> println("Gross, man!")
    null -> println("You gotta pick something")
    else -> println("Never heard of that, but you do you")
  }
}
```
Note the function parameter `flavor` with the `String?` type. This does not mean the parameter is optional! It only means that it is allowed to be `null`. So `null` would have to be explicitly passed into this function.

## 3.8 Using 'try'

Just like `if`, `try` is an expression in Kotlin.

```kotlin
val age: Int? = try {
  Integer.parseInt(person.age)
} catch (e: NumberFormatException) {
  null
}
```

## 3.9 Kotlin's looping constructs

`while` and `do...while` are the same as Java, as the same as JS.

But `for` loops are different than Java/JS.

First, lets look at the way a *range* is defined in Kotlin:

```kotlin
var numsFromOneToTen = 1..10
```
As you can see the default incrementor is 1. This is inclusive of 1 and 10.

In Kotlin, using this range in a `for` loop:

```kotlin
for (i in 1..10) {
  // i = 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
}
```

We can also use `step` to define the interval of incrementation:
```kotlin
for (i in 1..10 step 2) {
  // i = 1, 3, 5, 7, 9
}
```

We can also use `downTo` to indicate we should start from the end and go down to a certain point.
```kotlin
for (i in 10 downTo 5 step 2) {
  // i = 10, 8, 6
}
```
Note you can't just do `10..5`, you think you'd be able to go in the opposite direction, but you can't.

But what if you want your classic Java/JS "half-closed" range, inclusive of the beginning, exclusive of the end? Well you can use `until`:

```kotlin
for (i in 1 until 5) {
  // i = 1, 2, 3, 4
}
```
(Not sure if using `until` in reverse is possible.)

You can create a more explicit list using `listOf`, rather than using a range:

```kotlin
var retiredJerseyNumbers = listOf(2, 23, 30, 44)

for (number in retiredJerseyNumbers) {
  // stuff
}
```

...and you can even extend this loop to provide you the index:
```kotlin
var retiredJerseyNumbers = listOf(2, 23, 30, 44)

for ((index, number) in retiredJerseyNumbers.withIndex()) {
  // stuff
}
```
The course sort of pulls this out of nowhere, the first indication of a map(!), in order to show you that you can use a `for` loop on a map. But the `TreeMap` he provides doesn't actually work, seems like it's from a Java library. Did some research and found this:
https://www.deadcoderising.com/how-to-create-maps-in-kotlin-using-5-different-factory-functions-2/

```kotlin
var ages = mutableMapOf("Jeff" to 34, "Dana" to 29)
ages["Jeff"] = 35
ages["Dana"] = 30

for((name, age) in ages) {
  println("$name is $age years old")
}
```

Back to ranges real quick, i.e. `..`. You can implement a range over characters too, i.e. `a..z`.

"In fact, you can have a range over anything that implements the `Comparable` interface."

## 3.10 Kotlin's support for exceptions

Kotlin uses *unchecked exceptions* (I think this means uncaught exceptions, like JavaScript). So we don't have to specify if any class throws an exception.

Like JS and JavaScript, it supports a `finally` after the end of try/catch.

Look more into how Kotlin can use Java libraries. For instance `FileReader`
