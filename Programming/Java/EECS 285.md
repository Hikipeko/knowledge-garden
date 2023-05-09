语法，面向对象，接口，容器，异常，泛型，反射，注解，I/O

### Syntax

Primitive types: boolean, char, short, int, long, float, double, byte.

Each primitive type has a class wrapper.

Integer, Boolean, etc.

#### Enum

Can have fields and methods.

```java
enum Planet {
	MERCURY(3.303e+23, 2.4397e6),
	VENUS(4.869e+24, 6.0518e6),
	EARTH(5.976e+24, 6.37814e6),
	MARS(6.421e+23, 3.3972e6),
	JUPITER(1.9e+27, 7.1492e7),
	SATURN(5.688e+26, 6.0268e7),
	URANUS(8.686e+25, 2.5559e7),
	NEPTUNE(1.024e+26, 2.4746e7);
    
	private final double mass; // in kilograms
	private final double radius; // in meters
    
	Planet(double mass, double radius) {
		this.mass = mass;
		this.radius = radius;	
	}
	public double mass() { return mass; }
	public double radius() { return radius; }
}

public static void main (String[] args) {
    for (Planet planet : Planet.values()) {
        System.out.println("Planet " + planet + " has mass " 
                           + planet.mass() + " and radius " + planet.radius());
    }
}
```

#### ArrayList

Similar to vector in c++.



### Objects

Encapsulation, Inheritance, Polymorphism

Declare a non-primitive variable declares an object reference.

Must use new to obtain useable memory for an object.

```java
Rectangle rec = new Rectange();
```

Java passes all parameters by value: changing an object reference parameter in a method does not change the object reference in the calling function.

Java does not have the concept of a destructor.

Set an object reference to null when you no longer need it.

**Static**

Static fields are data associated with the class, not with individual instances.

Commonly used for constants (declared as final).

Static methods can only access static fields and static methods.

Should be called using class name.

**Final**

Must be assigned exactly once.

#### Inheritance

class A extends B

**super**

super keyword can be used to access a base-class member.

super(initial) call base-class constructor.

no access modifier (such as private): package-private:

accessible to others classes in the same package.

**Object Class**

Every class derives directly or indirectly from java.lang.Object

A reference of type Object can bind to an instance of any class.

equals()

toString()

#### Polymorphism

Allows a derived-class object to be used where a base-class object is expected.

Dynamic binding uses a derived class's method, even if the reference used to invoke he method is of a base-class type.

static methods use static binding, instance methods use dynamic binding

For an instance method, the method is (usually) bound at runtime.

```java
class A {
    static int sMethod() {
        return 0;
    }
}
class B extends A {
    static int sMethod() {
        return 1;
    }
}

A a = new A();
B b = new B();
a = b;
a.sMethod(); // return 0;
```

**Abstract class**

```java
abstract class Shape {
    private double x;
    private double y;
    ...
    public final double getX() { return x; }
    public abstract double area();
    public abstract double perimeter();
}
```

A method can be declared as final to prevent it from being overridden.

**@Override** 

Inform the compiler that we intended to override a method to prevent overload.

```java
@Override
public boolean equals(Object other) {
    if (other instanceof Rectangle) {
        Rectangle rec = (Rectangle) other;
        return rec.length == length && rec.width == width;
    }
    return false;
}
```

**Array**

```java
// define
int[] scores = new int[5];
int[] scores = { 100, 50, 21 };
// iterate
for (int item : score) {
    System.out.println(item);
}
// copy
int[] scores2 = scores.clone();
// multidimentional arrays
int[][] matrix = new int[3][4];

Array.sort(strings, (x, y) -> x.length() - y.length)
```

### Exceptions

 Amechanism for seperating error detection and error handling.

```java
try {
    // some code
} catch (Exception exc) {
    // error handling
}
```

 <img src="1.png" style="zoom:33%;" />

A class that derivs from Exception but not from RuntimeException is a checked exception.

When an exception is thrown, the current execution is stopped.

If the code within the catch completes normally, execution proceeds past the try/catch.

A common pattern is to define a hierachy of exceptions.

```java
public static void main(String[] args) {
	BankAccount account = new BankAccount(100);
	try {
		account.withdraw(-50);
		System.out.println("Success!");
	} catch (InvalidBalanceException exc) {
		System.out.println("Balance exception");
	} catch (BankException exc) {
	System.out.println("Bank exception");
} catch (Exception exc) {
	System.out.println("Generic exception");
}
	System.out.println(account.balance);
}
```

### Interface

Interface only have:

1. Abstract methods
2. Static methods
3. Default methods
4. Static final fields

A class implements an interface must provide implementations for each of the interface's abstract methods.

```java
interface Fuelable {
    int refuel();
}
interface SkyTraverlable {
    void fly();
}
class Helicopter extends Transport implements SkyTravelable, Fuelable {
    public void fly() {
        System.out.println("Helicopter fly");
    }
    public int refuel() {
        return 100;
    }
}
```

References can be of interface type.

```java
class Fueler {
    int gallonsOfFuel = 500;
    System.out.println("Provide fuel");
    gallonsOfFuel -= vehichle.refuel();
}
```

**Nested Classes**

A static nested class is defined within a static context.

An inner class is defined within a non-static context.

**Anonymous Classes**

```java
Arrary.sort(strings, new Comparator<String>() {
    public int compare(String x, String y) {
        return x.length() - y.length();
    }
});
```

### Container

**Generics**

Similar to template in c++.

```java
// in c++
// template <typename T>

// Only supports class that extends Shape
class OrderedPair<DataType extends Comparable<DataType>> {
    ...
}
// allows DataType ot implement Comparable<?>, where ? is a supertype of DataType
class OrderedPair<DataType extends Comparable<? super DataType>> {
	...
}
GenericClass<String> strObj = new GenericClass<>();
String str = strObj.method("foo");
```

Methods can also be generic

```java
static <T extends Comparable<? super T>> T max(T x, T y) {
    return x.compareTo(y) > 0 ? x : y;
}
```

### I/O

Scanner

```java
import java.util.Scanner;
class InputDemo {
    public static void main(String[] args) {
        int[] ssnChunks = new int[3];
        Scanner input = new Scanner(System.in);
        System.out.println("Enter First Middle Last:");
        String first = input.next();
        String middle = input.next();
        String last = input.next();
        System.out.println("Enter your SSN as XXX-XX-XXXX:");
        input.useDelimiter("[-\n]");
        for (int i = 0; i < 3; i++) {
            ssnChunks[i] = input.nextInt();
        }
        // pad with z
        System.out.printf("Last Name: %s, First & Middle: %s %s, "
                         + "SSN: %03d-%02d-%04d\n", last, first, middle, ssnChunks[0],
                         ssnChunks[1], ssnChunks[2]);
    }
}
```

Use Scanner to parse data from a string

like cstringstream

```java
static void parseLine(String line) [
    int id = -1;
    String nam e= "N/A";
    double price = -1;
    Scanner input = new Scanner(line);
    if (input.hasNextInt()) {
        id = input.nextInt();
    } else { error(); }
    if (input.hasNext()) {
        name = input.next();
    } else { error(); }
    if (input.hasNextDouble()) {
        price = input.nextDouble();
    } else { error(); }
    if (input.hasNext()) { errorExtra(); }
]
```

**File**

Any resource that implements the **AutoCloseable** interface can be used with a try-with-resource.

```java
public static void main(String[] args) {
    try (Scanner fileData = new Scanner(new File("productList.txt"))) {
        while (fileData.hasNext()) {
            parseLine(fileData.nextLine());
        }
    } catch (FileNotFoundException exc) {
        System.out.println("Product list file not found");
    }
}
```

**PrintStream**

```java
try (PrintStream output = new PrintStream("productOut.txt")) {
    while (fileData.hasNext()) {
        String line = fileData.nextLine();
        ...
        output.println(name + id + price);
    }
}
```





### Reflection

### Annotation