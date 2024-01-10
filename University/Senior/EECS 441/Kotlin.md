## K0 Layout

#### Project Structure and Manifest file

* `/manifests/AndroidManifest.xml`: general app settings
* `/java/PACKAGE_NAME`: app source code
* `/res/layout`: interface XML documents
* `/res/values`: constants for strings and other
* Gradle Scripts: build scripts and external dependencies

#### Activity and Activity Lifecycle

Each activity controls a single screen layout. 

* **ON_CREATE**: required to be implemented. Where `setContentView(view.root)` binds the Activity to a View.
* **ON_PAUSE**: last chance to save persistent data.

![[Pasted image 20230117164122.png]]

#### Launching Activity and Intent

Can launch an activity by calling `startActivity(Intent(this, ActivityName))`. We can bind the function with an action (e.g. onClick).

##### Intents

An Intent is a messaging object through which Android app components communicate and interact with each other. It allows tasks to be completed asynchronous.

Constructor: `Intent(Context, Class)`.

##### Context

Context is the interface to app's execution environment.

#### View, View Layout, View Binding

View/widget: a single view element.

ViewGroup, Layout: used interchangeably to refer to a view hierarchy.

**Density-Independent Pixel (DP)** automatically converts to the right number of pixels for screen.

**Edges**: start = left, end = right. Use "start" and "end".

##### Margin and Padding

![[Pasted image 20230117174021.png]]

A layout is only a description, we need a model to instantiate, or to **inflate** the layout. The **ViewBinding** library automatically generates a **binding object** for every layout file in your app.

#confused 



### K1 List View

#### Variables

Kotlin is a statically and strongly typed language (like Java).

* **val** defines an immutable variable
* **const val** defines a compile-time constant
* **var** defines a mutable variable

##### String

A sequence of **Char**. A Char in Kotlin is not a byte, but a variable-length Unicode.

##### Array

Works like a Java array.

##### Collection Types

* **List**: an immutable array interface
* **MutableList**: a mutable array interface, can add and remove
* **ArrayList**: a class implements the MutableList interface
* **Set**: unordered & unique
* **Map**
* **HashMap**: a class implementing MutableMap

##### Nullables

Denoted by ? following a type, e.g. Int?. Either has a value or can be null. Used for safety reason.

##### Control Flow

```kotlin
val max = if (a > b) a else b

// for loop
for (i in 0..3) { println(i) } // 0, 1, 2, 3
for (i in 6 downTo 0 step 2) { println(i) } // 6, 4, 2, 0
for ((index, value) in stuff.withIndex())
```

##### Functions

```kotlin
fun mult(x: Int, y: Int): Int { return x * y }
fun mult_equiv(x: Int, y: Int) = x * y
fun hello() { println("Hello world!") } // return type inferred to be Unit (void)
```

#### Displaying a List

* Model: a `List` of data items
* View: the `ListView` class, a subclass of `AdapterView`
* Controller: the `ArrayAdapter` class



## K3 Lambda Expressions

