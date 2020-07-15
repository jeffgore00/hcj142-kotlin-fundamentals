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
class Person(var name: String) {}
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

(Look more into how Kotlin can use Java libraries. For instance `FileReader`.)


## 4.1 Functions In Kotlin - Introduction

In Kotlin, functions don't need to be declared as part of class. In Java, no functions can exist outside of a class definition. Static functions still belong to a class.

In Kotlin, functions can have default parameters (not possible in Java).

And you can have named parameters - not even possible with JS (closest would be passing object as prop).


## 4.2 Declaring functions

The function signature must have an input type and a return type, if applicable. But if there are no parameters or no return, then we don't have to supply the type (instead of Java, where you'd have to say `void`).

Here's our first ever example with a return value:

```kotlin
fun display(message: String) : Boolean {
    println(message)
    return true
}
```

Just like Java, functions can be kept in `package`s. (Java has a default package, look into this). The package directive must match the file location.

```kotlin
package utils
// "utils" should be a folder that is a sibling to the `MainKt.class` that is executed by the JVM

// function/class declarations here.
```

Kotlin also supports function expressions, similar to JS:
```kotlin
// remember `if` is an expression in Kotlin
fun max(a: Int, b: Int): Int = if (a > b) a else b
```

## 4-3 Interoperability with Java

Let's say we compiled this `utils` package mentioned above, which had a filename called `Display.kt`, which in turn had a stand-alone function (no class) called `display`.

And then let's say we were writing a Java program that wanted to utilize this Kotlin code. We should know that the Kotlin compiler will transform this:

```kotlin
/* utils/display.kt */
package utils

fun display(str: String) : Boolean {
  println(str)
  return true
}
```
...into this:

```java
package utils;

class DisplayKt {
  static boolean display(String str) {
    // etc
  }
}
```

OK, so let's get into some Java code that would use our Kotlin package.

```java
import rsk.*;

public class App {
  public static void main(String[] args) {
    DisplayKt.display("Hello, world from Java")
  }
}
```

What if we don't want our Kotlin export to be called `DisplayKt`? Well we can use a special annotation in our Kotlin files to name the generated class:

```kotlin
/* utils/display.kt */
@file:JvmName("DisplayUtils")

package utils

fun display(str: String) : Boolean {
  println(str)
  return true
}
```

## 4-4 Default Parameters

Example of a function with default parameters:

```kotlin
fun connect(addr: URI, useProxy: Boolean = true) : Boolean {

}
```

But because Java doesn't have default parameters, how can we compile compatible code? Let's use the `@JvmOverloads` annotation. This will make the Kotlin compiler generate overloads for the function.

In this example, this annotation will result in two Java functions - one that takes one argument, and one that takes two. And remember this annotation is like a decorator, so it applies only to the function directly underneath it.

```kotlin
@JvmOverloads
fun connect(addr: URI, useProxy: Boolean = true) : Boolean {

}
```

## 4-5 Named Parameters

Remember this function?

```kotlin
@JvmOverloads
fun connect(addr: URI, useProxy: Boolean = true) : Boolean {

}
```

Here's how you can call it with one parameter actually named. This makes the code meaning clearer:

```kotlin
connect(URI(/* some URL */), useProxy = false)
```

Note this is only possible for this configuration. If you tried to do the opposite:

```kotlin
// ERROR! "Mixing named and positioned arguments is not allowed"
connect(addr = URI(/* some URL */), false)
```
You would get an error. The logic is: "once we use a named parameter in an argument, we must use named parameters for all subsequent arguments."

But what's cool about named parameters is that if you name ALL of your parameters, then you can pass them in any order you please.

Java doesn't support named parameters. Neither does JS.


## 4.6 Extension Functions

These are not in Java, they're in C# though.

With this, you can "add" functions to classes you don't own. This cuts down on utility classes and makes code easier to read.

Think about how you can alter a JS prototype to have new methods. In this example we extend the `String` class with a new method:

```kotlin
fun main(args: Array<String>) {
    val text = "With     lots  \t  of whitespace"
    printLn(text.myCustomSpaceConsolidator())
}

fun String.myCustomSpaceConsolidator() : String {
  var regex = Regex("\\s+")
  return regex.replace(this, " ")
}
```

## 4.7 Infix functions

Infix functions are:

- member or extension functions
- have a single parameter
- marked with the `infix` keyword

..and they allow you to call the function only with the name, without the dot or the parentheses. The method would belong to the object on the left hand side, and would take the expression on the right hand side as its sole argument.

Here's an example:

```kotlin
class State(var desc: String) {}

infix fun State.mergedWith(otherState: State) : State {
    return State("${this.desc}; ${otherState.desc}")
}

fun main(args: Array<String>) {
    val clientState = State("browser: chrome")
    val serverState = State("featureService: on")

    // Here it is!
    val mergedState = clientState mergedWith serverState
    println(mergedState.desc)
}

```
Doesn't `mergedWith` in the above example look almost like a custom operator? Well, it sort of is. And you can leverage a C++ feature here and go further and actually overload an operator. Let's say you wanted the `+` operator to actually execute the task of `mergedWith`. But you'd actually have rename `mergedWith` to `plus` which is what the operator is named.

You could do this with the `operator` keyword:

```kotlin
class State(var desc: String) {}

// Note "operator"
infix operator fun State.plus(otherState: State) : State {
    return State("${this.desc}; ${otherState.desc}")
}

fun main(args: Array<String>) {
    val clientState = State("browser: chrome")
    val serverState = State("featureService: on")

    // Note its use here:
    val mergedState = clientState + serverState
    println(mergedState.desc)

    // and this still works because the prior implementation is an overload, not replacing the function entirely.
    println(2 + 2)
}
```
Unlike C++, Kotlin limits which operators you can overload.

Why would you want to do this? For one, this makes Domain Specific Languages (DSLs) easier to write.


## 4.8 Tail Recursive Functions

Kotlin supports tail recursion. You would have to mark this as `tailrec`, and as long is it follows the right form and is truly tail recursive, the Kotlin will optimize this by turning it into a loop inside the bytecode.

Fibonacci in Kotlin with tail recursion:

```kotlin
// Calculates the nth fibonacci number
tailrec fun fib(n: Int, a: BigInteger, b: BigInteger): BigInteger {
  return if (n == 0) b else fib(n - 1, a + b, a)
}
```

## 5.1 Programming with Types Introduction
(nothing)

## 5.2 Interfaces

Here you go:

```kotlin
interface Time {
    fun setTime(hours: Int, mins: Int = 0, secs: Int = 0)
}

class AlienTime : Time {
    override fun setTime(hours: Int, mins: Int, secs: Int) {}
    fun setNextMoltingHour(hour: Int) {}
}
```
Note Kotlin has no `implements` or `extends` keyword, the colon `:` suffices in the class defintion.

And also IMO the `override` is a little confusing, but you need to use that keyword. It's not really an override in terms of changing the implementation, it's satisfying ther requirements of the interface and *following* the implementation. Without `override` you get this error: "'setTime' hides member of supertype 'Time' and needs 'override' modifier."

The only time you wouldn't need to do this is if the interface had a default implementation:

```kotlin
interface Time {
    fun printSomethingSimple() {
        println("now")
    }
}

class AlienTime : Time {
  // this has access to the `printSomethingSimple` method
}
```

What if you have a class that implements multiple interfaces? Separate them with a comma, i.e. `class Sample : A, B`

And what if each of those interfaces has a method of the same name with a default implementation? In which case, you must `override`, even if you want to call a default implementation, which you would do via `super`:

```kotlin
interface A { fun doSomething() = {} }
interface B { fun doSomething() = {} }

class Sample : A, B {
  override fun doSomething() = {
    super<A>.doSomething()
  }
}
```

## 5-3 Defining Classes in Kotlin

In Kotlin, classes are `public` by default. And they're `final` by default, meaning they can't be used for inheritance (in Java, they're `open` by default, the opposite, but a best practice in Java is to always use `final`).

Methods are `final` by default as well, meaning they can't be overridden. To counteract that, we use the `open` keyword, which allows either a class to be implemented or a method to be overridden.

```kotlin
open class Person {
    var firstName: String = ""
    var lastName: String = ""
    fun getName() : String = "$firstName $lastName"
}

class Student : Person() {
    override fun getName() : String { return "student. my name doesnt matter" }
}
```
Note that because we're extending a class rather than an interface, the parent class is invoked like a function.

Kotlin supports `abstract` classes as well. What makes them different from  interfaces is that they can store state. And like an interface, if a method is defined, it must be used.

```kotlin
abstract class Person {
    var firstName: String = ""
    var lastName: String = ""
    open fun getName() : String = "$firstName $lastName"
    abstract fun getAddress() : String
}

class Student : Person() {
    // overriding because I want a custom implemention, otherwise I don't have to do this. The parent function is not abstract, has an implementation.
    override fun getName() : String { return "student. my name doesnt matter" }

    // overriding because I MUST provide an implementation for the abstract function, unless I wanted this function to be abstract as well
    override fun getAddress() : String { return "my moms house"}
}
```
Kotlin does not use packages for visibility scoping; there's a class identifier called `package-private` in Java but Kotlin doesn't use that. It does have something called `internal`, though - limiting visibilty of a class to only a "module" - look into that. And it still has `private` and `protected`.


## 5-4 Sealed classes

Like "enums on steroids", sealed classes are used to restrict class hierarchies. Classes are defined within the sealed class, meaning the sealed class only allows its innards to inherit it.

```kotlin
sealed class HumanAction {
  class Awaken : HumanAction()
  class FallAsleep : HumanAction()
  class Eat(val food: String) : HumanAction()
}

fun handleHumanAction(action: HumanAction) {
  when (action) {
    is HumanAction.Awaken -> println("Awake")
    is HumanAction.FallAsleep -> println("Asleep")
    is HumanAction.Eat -> println(action.food)
  }
}
```

## 5-5 Providing Constructors

In Java, the constructor would be a function within the class where the name is the same as the class name. (I think.)

In Kotlin, the constructor is just like a function argument, except with the additional `val` or `var` qualifier, which equates to a read-only vs. mutable property, sensibly.

```kotlin
class Person(var name: String) {}
```

If you wanted, though, you could omit the `var`/`val` from the arguments parentheses provided you provide it later with an `init` call:

```kotlin
class Person(name: String) {
  var name: String
  init {
    this.name = name
  }
}
```
Or if you name your variables with an underscore, you can similarly defer the `val` vs `var` designation, but why would you?:

```kotlin
class Person(_name: String) {
  var name = _name
}
```

You can also define secondary constructors within the class.

```kotlin
class Person(var name: String) {
    var age : Int? = null
    constructor(name: String, age: Int) : this(name) {
        this.age = age
    }
}
```
Note the `this(name)` in there: "If the class has a primary constructor, each secondary constructor needs to delegate to the primary constructor."

But we may as well use default values instead:

```kotlin
class Person(var name: String, var age: Int? = null) {}
```

Here's how you would pass constructor arguments up to a parent class:

```kotlin
class Student(firstName: String, lastName: String) : Person(firstName, lastName) {}

// or
class Student: Person {
    constructor(name: String) : super(name)
}
```
Note there is no `var` or `val` in the child class's constructor, because those arguments already exist in the parent constructor.

Again, no `new` keyword used in Kotlin to invoke the constructor.


## 5-6 Data Classes

"We frequently create classes whose main purpose is to hold data. In such a class some standard functionality and utility functions are often mechanically derivable from the data."

Kotlin's data classes are like standard classes, but they also come in with built in overrides:
  - `equals` (which is what the `==` operator calls under the hood): deep object equality rather than comparing by reference
  - `toString`: string representation of all the object's key/value pairs, rather than printing the object's address in memory
  - `copy`: not an override, but allows an object to be cloned, with the only changed attributes being the arguments passed in.

```kotlin
data class Product(val style: Int, val color: Int, val size: Int)

fun main(args: Array<String>) {
    var whiteShirt = Product(1, 1, 2)
    println(whiteShirt.toString()) // Product(style=1, color=1, size=2)

    var blackShirt = whiteShirt.copy(color = 4)
    println(blackShirt.toString()) // Product(style=1, color=4, size=2)

    var blackShirtCopy = blackShirt.copy()
    println(blackShirt == blackShirtCopy) // true
}
```

## 6-1 Companion Objects Introduction

## 6-2 Using the `object` keyword

Kotlin itself does not use the `static` keyword. But we can have singletons. The `object` keyword does this - defines a class and creates an instance of that class in one go, creating a singleton.

```kotlin
object DataStoreManager {
  var mongoConnection : MongoConnection? = null;
  var redisConnection : RedisConnection? = null;

  fun connect() {
    // some logic
  }
}
```
Note that there is no constructor because the construction occurs in this definition, and nothing else would have the power to construct it.


## 6-3 Extending objects

It is possible for an `object` to implement an existing class or interface.

```kotlin
object DataStoreManager : LoansDataStore {
  fun connect() {
    // some logic
  }
}
```

And they can be declared inside of other classes:

```kotlin
class LoansDataStore {
  object mongoConnection : MongoConnection
  object redisConnection : RedisConnection

  fun connect() {
    // some logic
  }
}
```

## 6-4 Companion objects

Kotlin classes do not have `static` members. So how do you go about adding static methods to a Kotlin class?

Enter the `companion object`. What makes them companions is that they are used within classes and attached to their enclosing class as a property.

## 6-5 Using companion objects

Here's how you'd create the equivalent of a static method. Let's say you missed JS and you wanted to create a `Math.floor` function.

```kotlin
class Math() {
    companion object {
        fun floor(number : Double) : Double {
            // I would have said kotlin.math.floor(), but the syntax highlighting here has a problem with it, even though its valid.
            return nativeFloor(number)
        }
    }
}
```

So, you see how that is essentially a static method - there is only one instance of this function on the class.

But to make this regarded as a static method by Java, you would have to add the `@JvmStatic` annotation:

```kotlin
class Math() {
    companion object {
        @JvmStatic
        fun floor(number : Double) : Double {
          // implementation
        }
    }
}
```
Look also into how companion objects help with factories.


## 7-1 Using High Level Functions - Intro

The "strategy pattern" is a gang of four pattern that allows an algorithm's behavior to be selected at runtime.


## 7-2 Using Anonymous Classes to Implement Functionality

You can pass inline / anonymous objects to functions - sort of. They still need to have a type. See this below example.

Keep in mind this is "the OO method", which is evidently the way things are done in Java as well. This is just the setup for a later module that shows that a callback is better.

```kotlin
interface OnComplete {
    fun execute(responseData: String)
}

class ApiFetcher {
    fun fetch(url: String, onComplete: OnComplete) {
        var responseData : String = "initial value";
        // responseData = do some HTTP call
        onComplete.execute(responseData)
    }
}

// Here, an anonymous instance of the `OnComplete` interface is passed to the `fetch` method via the `object` keyword.
fun main(args: Array<String>) {
    var fetcher = ApiFetcher()
    fetcher.fetch("http://coolguy.com/api/guys", object : OnComplete{
        override fun execute(responseData: String) {
            println(responseData)
        }
    })
}
```

## 7-3 Introducing Higher Order Functions

Let's reimplement that with a callback.

```kotlin
class ApiFetcher {
    fun fetch(url: String, callback: (String) -> Unit) {
        var responseData : String = "initial value";
        // data = do some HTTP call
        callback(responseData)
    }
}

fun main(args: Array<String>) {
    var fetcher = ApiFetcher()

    // Below is our lambda function passed in as our callback. Note that it's outside of the parentheses. That's the IDE stylistic preference, though you *could* pass it within the parens like you would a JS function.
    fetcher.fetch("http://coolguy.com/api/guys") { responseData -> println(responseData) }
}
```

## 7-3 Using Higher Order Functions In Kotlin

But wait, it can get even more concise. If the lambda takes just one parameter, then you can use the keyword `it` to refer to the sole parameter. Or you could just use the function reference syntax `::`.

```kotlin
// so instead of
fetch("http://coolguy.com/api/guys") { responseData -> println(responseData) }

// you can do:
fetch("http://coolguy.com/api/guys") { println(it) }

// and you can even go further
fetch("http://coolguy.com/api/guys", ::println)
```

## 7-4 Closures

Kotlin lambdas can mutate state, unlike Java 8+ lambdas. (check this)

## 7-5 With and Apply

Let's take a look at a simple example of a program that creates a meeting:

```kotlin
class Meeting {
    var title: String = ""
    var date: Date = Date()
    var attendees = mutableListOf<String>()

    fun sendInvites() {}
}

fun main(args: Array<String>) {
    var meeting = Meeting()
    meeting.title = "A New World Order"
    meeting.date = Date(2020,12,30)
    meeting.attendees.add("Jeff")
    println("Meeting created")
}
```
This works fine. But using the `with` function (part of the Kotlin standard library, you can do this:

```kotlin
fun main(args: Array<String>) {
    var meeting =  Meeting()
    with(meeting) {
        title = "A New World Order"
        date = Date(2020,12,30)
        attendees.add("Jeff")
    }
    println("Meeting created")
}
```
What's cool about `with` is that the area in brackets is actually a lambda function, receiving the `Meeting` instance as its `this` value. So when it makes those assignments, it is actually assigning those properties on the `meeting`.

What if you want to return that `meeting`? Then you could use `apply`, which has similar syntax, but is called on the object itself, and returns the object itself.

```kotlin
fun main(args: Array<String>) {
    var meeting =  Meeting()
    meeting.apply {
        title = "A New World Order"
        date = Date(2020,12,30)
        attendees.add("Jeff")
    }.sendInvites()
}
```
Extra points for refactoring here:

```kotlin
fun main(args: Array<String>) {
    Meeting().apply {
        title = "A New World Order"
        date = Date(2020,12,30)
        attendees.add("Jeff")
    }.sendInvites()
}
```

## 8.1 - Filtering and Sorting Intro

## 8.2 - Filter and Map in Kotlin

An example of `filter`, which is avaiable on Kotlin collections. Note the lambda.

```kotlin
  val ints = listOf(1,2,3,4,5)
  val smallInts = ints.filter{ it < 4 }
```

And here's `map`:

```kotlin
  val ints = listOf(1,2,3,4,5)
  val intsSquared = ints.map{ it * it }
```

And they can be chained:

```kotlin
  val ints = listOf(1,2,3,4,5)
  val intsSquared = ints.map{ it * it }.filter{ it < 15 }
```

> Sidenote: there's a `startsWith` method on the `String` class.

Note that with `map` we can return a new type:

```kotlin
  val people = listOf(Person("Jeff"), Person("Dana"))
  val names = people.map{ it -> it.name }
```
