# Scala

### Evaluation Rules
* Call by value: evaluates the function arguments before calling the function
* Call by name: evaluates the function first, and then evaluates the arguments if need be

```scala
def square(x: Double)    // call by value
def square(x: => Double) // call by name
def myFct(bindings: Int*) { ... } // bindings is a sequence of int, containing a varying # of arguments
```

### REPL
```bash
$ scala
$ sbt
> console
```

### Hello World
Create a runnable application:
```scala
object Hello {
  def main(args: Array[String]) = println("Hello world")
}

// or
object Hello extends App {
  println("Hello World")
}
```

### Basics
check object type: `obj.getCLass`

### Functions
```scala
def square(i: Int): Int = Math.pow(i, 2).toInt  // regular
val square = (i: Int) => Math.pow(i, 2).toInt   // anonymous
```

* map

    ```scala
    // square values in list
    val numbers = List(1, 2, 3)
    numbers.map(Math.pow(_, 2).toInt)
    numbers.map((i: Int) => Math.pow(i, 2).toInt)
    numbers.map(square)
    ```

* filter

    ```scala
    def isEven(i: Int): Boolean = i % 2 == 0
    numbers.filter(isEven _)
    numbers.filter((i: Int) => i % 2 == 0)

    // Map type
    val names = Map("steve" -> 100, "bob" -> 101, "joe" -> 201)
    names.filter({case (key, value) => value < 200})
    ```

* zip

    ```scala
    numbers.zip(List("a", "b", "c"))         //  List[(Int, String)] = List((1,a), (2,b), (3,c))
    ```

* flatten

    ```scala
    List(List(1, 2), List(3, 4)).flatten     // List[Int] = List(1, 2, 3, 4)
    ```

* flatMap

    ```scala
    // equivalent
    List(List(1, 2), List(3, 4)).flatMap(x => x.map(_ * 2))
    List(List(1, 2), List(3, 4)).flatten.map(x => x.map(_ * 2))
    ```

* groupBy

    ```scala
    val raptors = List("Golden Eagle", "Bald Eagle", "Prairie Falcon",
                       "Peregrine Falcon", "Harpy Eagle", "Red Kite")
    val kinds = raptors.groupBy {
       case bird if bird.contains("Eagle")  => "eagle"
       case bird if bird.contains("Falcon") => "falcon"
       case _ => "unknown"
    }

    val words = List("one", "two", "one", "three", "four", "two", "one")
    val counts = words.groupBy(w => w).mapValues(_.size)
    ```

### Higher order functions
These are functions that take a function as a parameter or return functions
```scala
// sum() returns a function that takes two integers and returns an integer
def sum(f: Int => Int): (Int, Int) => Int = {
  def sumf(a: Int, b: Int): Int = {...}
  sumf
}

// same as above. Its type is (Int => Int) => (Int, Int) => Int
def sum(f: Int => Int)(a: Int, b: Int): Int = {...}

// Called like this
sum((x: Int) => x * x * x)          // Anonymous function, i.e. does not have a name
sum(x => x * x * x)                 // Same anonymous function with type inferred

def cube(x: Int) => x * x * x
sum(x => x * x * x)(1, 10) // sum of cubes from 1 to 10
sum(cube)(1, 10)           // same as above
```


### Currying
Converting a function with multiple arguments into a function with a single argument that returns another function.
```scala
def f(a: Int, b: Int): Int // uncurried version (type is (Int, Int) => Int)
def f(a: Int)(b: Int): Int // curried version (type is Int => Int => Int)
```

### Classes
```scala
class MyClass(x: Int, y: Int) {           // Defines a new type MyClass with a constructor
  require(y > 0, "y must be positive")    // precondition, triggering an IllegalArgumentException if not met
  def this (x: Int) = { ... }             // auxiliary constructor
  def nb1 = x                             // public method computed every time it is called
  def nb2 = y
  private def test(a: Int): Int = {...}   // private method
  val nb3 = x + y                         // computed only once
  override def toString =                 // overridden method
      member1 + ", " + member2
  }

new MyClass(1, 2) // creates a new object of type
```
`this` references the current object, `assert(<condition>)` issues `AssertionError` if condition is not met. See `scala.Predef` for `require`, `assume` and `assert`.

### Class hierarchies
```scala
// abstract class with abstract methods
abstract class TopLevel {
  def method1(x: Int): Int
  def method2(x: Int): Int = {...}
}

class Level1 extends TopLevel {
  def method1(x: Int): Int = {...}
  override def method2(x: Int): Int = {...} // TopLevel's method2 needs to be explicitly overridden
}

// defines a singleton object. No other instance can be created
object MyObject extends TopLevel {...}
```

### Class Organization
All members of packages `scala` and `java.lang` as well as all members of the object `scala.Predef` are automatically imported.
```scala
// Classes and objects are organized in packages
package myPackage

// Referenced through import statements
import myPackage.MyClass
import myPackage._
import myPackage.{MyClass1, MyClass2}
import myPackage.{MyClass1 => A}

// Directly referenced in the code with the fully qualified name
new myPackage.MyClass1
```

General object hierarchy:
* `scala.Any` base type of all types. Has methods `hashCode` and `toString` that can be overridden
* `scala.AnyVal` base type of all primitive types. (`scala.Double`, `scala.Float`, etc.)
* `scala.AnyRef` base type of all reference types. (alias of `java.lang.Object`, supertype of `java.lang.String`, `scala.List`, any user-defined class)
* `scala.Null` is a subtype of any `scala.AnyRef` (`null` is the only instance of type `Null`), and `scala.Nothing` is a subtype of any other type without any instance.

### Traits
```scala
trait Planar { ... }
class Square extends Shape with Planar
```

### Collections

Base Classes
- [`Iterable`](http://www.scala-lang.org/api/current/index.html#scala.collection.Iterable): collections you can iterate on
- [`Seq`](http://www.scala-lang.org/api/current/index.html#scala.collection.Seq): ordered sequences
- [`Set`](http://www.scala-lang.org/api/current/index.html#scala.collection.Set)
- [`Map`](http://www.scala-lang.org/api/current/index.html#scala.collection.Map): lookup data structure

    ```scala
    val myMap = Map("I" -> 1, "V" -> 5, "X" -> 10)  // create a map
    myMap("I")      // => 1
    myMap("A")      // => java.util.NoSuchElementException
    myMap get "A"   // => None
    myMap get "I"   // => Some(1)
    myMap.updated("V", 15)  // returns a new map where "V" maps to 15 (entry is updated)
                            // if the key ("V" here) does not exist, a new entry is added
    ```

Immutable Collections
- [`List`](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.List): linked list, provides fast sequential access

    ```scala
    val fruitList = List("apples", "oranges", "pears")
    // Alternative syntax for lists
    val fruit = "apples" :: ("oranges" :: ("pears" :: Nil)) // parens optional, :: is right-associative
    fruit.head   // "apples"
    fruit.tail   // List("oranges", "pears")
    val empty = List()
    val empty = Nil
    ```

- [`Stream`](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.Stream): same as `List`, except that the tail is evaluated only on demand

    ```scala
    val xs = Stream(1, 2, 3)
    val xs = Stream.cons(1, Stream.cons(2, Stream.cons(3, Stream.empty))) // same as above
    (1 to 1000).toStream // => Stream(1, ?)
    x #:: xs // Same as Stream.cons(x, xs)
             // In the Stream's cons operator, the second parameter (the tail)
             // is defined as a "call by name" parameter.
             // Note that x::xs always produces a List
    ```

- [`Vector`](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.Vector): array-like type, implemented as tree of blocks, provides fast random access

    ```scala
    val nums = Vector("louis", "frank", "hiromi")
    // element at index 1, complexity O(log(n))
    nums(1)
    // new vector with a different string at index 2
    nums.updated(2, "helena")
    ```

- [`Range`](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.Range): ordered sequence of integers with equal spacing

    ```scala
    val r: Range = 1 until 5 // 1, 2, 3, 4
    val s: Range = 1 to 5    // 1, 2, 3, 4, 5
    1 to 10 by 3  // 1, 4, 7, 10
    6 to 1 by -2  // 6, 4, 2
    ```

- [`String`](http://docs.oracle.com/javase/1.5.0/docs/api/java/lang/String.html): Java type, implicitly converted to a character sequence, so you can treat every string like a `Seq[Char]`

    ```scala
    val s = "Hello World"
    s filter (c => c.isUpper) // returns "HW"; strings can be treated as Seq[Char]
    ```

- [`Map`](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.Map): collection that maps keys to values

    ```scala
    val imm_fruit_count = Map("apples" -> 4, "oranges" -> 5, "bananas" -> 6)
    imm_fruit_count("apples")
    ```

- [`Set`](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.Set): collection without duplicate elements

    ```scala
    val fruitSet = Set("apple", "banana", "pear", "banana")
    fruitSet.size    // returns 3: there are no duplicates, only one banana

    val s = (1 to 6).toSet
    s map (_ + 2)    // adds 2 to each element of the set
    ```

Mutable Collections
- [`Array`](http://www.scala-lang.org/api/current/index.html#scala.Array): Mutable arrays of fixed length. Scala arrays are native JVM arrays at runtime, therefore they are very performant

    ```scala
    val init_int = new Array[Int](10) //
    ```

- [`ArrayBuffer`](http://www.scala-lang.org/api/current/index.html#scala.collection.mutable.ArrayBuffer): Mutable arrays of varying length

    ```scala
    import scala.collection.mutable.ArrayBuffer
    val int_arr = ArrayBuffer[Int]()
    ```

- Scala also has mutable maps and sets; these should only be used if there are performance issues with immutable types

### Operations on sequences
```scala
val xs = List(...)
xs.length         // number of elements, complexity O(n)
xs.last           // last element (exception if xs is empty), complexity O(n)
xs.init           // all elements of xs but the last (exception if xs is empty), complexity O(n)
xs take n         // first n elements of xs
xs drop n         // the rest of the collection after taking n elements
xs(n)             // the nth element of xs, complexity O(n)
xs ++ ys          // concatenation, complexity O(n)
xs.reverse        // reverse the order, complexity O(n)
xs updated(n, x)  // same list than xs, except at index n where it contains x, complexity O(n)
xs indexOf x      // the index of the first element equal to x (-1 otherwise)
xs contains x     // same as xs indexOf x >= 0
xs filter p       // returns a list of the elements that satisfy the predicate p
xs filterNot p    // filter with negated p
xs partition p    // same as (xs filter p, xs filterNot p)
xs takeWhile p    // the longest prefix consisting of elements that satisfy p
xs dropWhile p    // the remainder of the list after any leading element satisfying p have been removed
xs span p         // same as (xs takeWhile p, xs dropWhile p)

List(x1, ..., xn) reduceLeft op    // (...(x1 op x2) op x3) op ...) op xn
List(x1, ..., xn).foldLeft(z)(op)  // (...( z op x1) op x2) op ...) op xn
List(x1, ..., xn) reduceRight op   // x1 op (... (x{n-1} op xn) ...)
List(x1, ..., xn).foldRight(z)(op) // x1 op (... (    xn op  z) ...)

xs exists p       // true if there is at least one element for which predicate p is true
xs forall p       // true if p(x) is true for all elements
xs zip ys         // returns a list of pairs which groups elements with same index together
xs unzip          // opposite of zip: returns a pair of two lists
xs.flatMap f      // applies the function to all elements and concatenates the result
xs.sum            // sum of elements of the numeric collection
xs.product        // product of elements of the numeric collection
xs.max            // maximum of collection
xs.min            // minimum of collection
xs.flatten        // flattens a collection of collection into a single-level collection
xs groupBy f      // returns a map which points to a list of elements
xs distinct       // sequence of distinct entries (removes duplicates)

x +: xs           // creates a new collection with leading element x
xs :+ x           // creates a new collection with trailing element x
```

### Pairs (Tuples)
```scala
val pair = ("answer", 42)   // type: (String, Int)
val (label, value) = pair   // label = "answer", value = 42
pair._1 // "answer"
pair._2 // 42
```

### For-comprehensions
Syntactic sugar for `map`, `flatMap` and `filter` operations on collections.

The general form is `for (s) yield e`
```scala
// list all combinations of numbers x and y
for (x <- 1 to 5; y <- 6 to 10)
  yield (x ,y)

// is equivalent to
(6 to 10) flatMap (x => (1 to 5) map (y => (x, y)))


for (i <- 1 until n; j <- 1 until i if isPrime(i + j))
    yield (i, j)

// is equivalent to
(1 until n).flatMap(i => (1 until i).filter(j => isPrime(i + j)).map(j => (i, j)))


for (x <- e1) yield e2           // e1.map(x => e2)
for (x <- e1 if f) yield e2      // for (x <- e1.filter(x => f)) yield e2
for (x <- e1; y <- e2) yield e3  // e1.flatMap(x => for (y <- e2) yield e3)
```

### Assertions
Is used to check the code of the function itself.
```scala
val x = sqrt(y)
assert(x >= 0)
```

### Operators
Infix notation
* `r.add(s)` can be replaced by `r add s`
* `myObject myMethod 1` is the same as calling `myObject.myMethod(1)`

# Examples
Newton's method for approximating square root. To compute sqrt(x):

1. start with an initial estimate y (let's pick y = 1).
2. repeatedly improve the estimate by taking the mean of y and x/y

```scala
object sqrt {

  def abs(x: Double) = if (x < 0) -x else x

  def sqrt(x: Double) = {

    def sqrtIter(guess: Double): Double =
      if (isGoodEnough(guess)) guess
      else sqrtIter(improve(guess))

    def isGoodEnough(guess: Double) =
      abs(guess * guess - x) / x < 0.001

    def improve(guess: Double) =
      (guess + x / guess) / 2

    sqrtIter(1.0)
  }

  def main(args: Array[String]) = {
    println(sqrt(args(0).toDouble))
  }
}
```

Recursion
factorial (not tail recrusive function)
```scala
def factorial(n: Int): Int =
  if (n == 0) 1
  else n * factorial(n - 1)
```

Word Count
```scala
val textFile = io.Source.fromFile("data/pg1661.txt").mkString
val counts = textFile.split("\\s+").groupBy(x => x).mapValues(x => x.length)
counts("Holmes")
```

# Akka
An actor framework written in Scala. It offers an asynchronous, callback-based Actor DSL. Akka doesn’t doesn’t provide lightweight threads but relies on JVM threads to schedule actors. Rather than a library, Akka is a full-service framework, covering everything from configuration and deployment to testing.
