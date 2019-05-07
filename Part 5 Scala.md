# Part 5: Scala

Scala is a hybrid language that combines two core programming paradigms: object orientation and functional programming. Spark, one of the fastest growing components in the Hadoop eco-system, was written in Scala.

Features of Scala:

1. Scala and Java share a common runtime platform.
2. Statistically typed.
3. Full support of OOP.
4. Full support of FP.
5. Elegant and flexible syntax.
6. Support for concurrency and synchronization.

***Normal Expressions***

```scala
//Evaluating expressions
5 + 3

//These res values (a shortened version of result) are sequentially numbered.
res0 * 10

//Multiple expressions and statements can be entered on a single line separated from each other by semicolons. The last expression or statement does not need to be terminated with a semicolon
val x = 10; val y = "hi there"; println(x); println(y)

//The alternative is to use a multiline string, which is enclosed with triple quotes:
val greeting = """She suggested reforatting the file
by replacing tabs(\t) with newlines(\n)
"Why do that?", he asked."""

val x,y,z = 0

val (a,b,c,d) = (1,2,3,4)

val `Var Space` = "test"

`Var Space`
```

***Type Conversions***

```scala
val s: Short = 10
//Short to Double
val d: Double = s

//Error higher rank to lower rank conversion
val s: Double = 10
val d: Short = s

//To convert from a higher ranked type to a lower ranked type, explicit conversion using the toType method is required as it is possible to lose data in this type of conversion

val myLong: Long = 20
val myInt: Int = myLong.toInt

val id = 100l //Long
val pi = 3.146 //Double
val pi = 3.146f //Float

val x = 5/2 //Int
val x = 5.0/2 //Double
val x = 5/2.0f //Float

val signature = "With Regards, \nYour friend" //String

val matched = (greeting == "Hello, World") //Boolean

val words = "cat " * 5

val number = "345".toInt

val number = "345".toDouble

val number = "345.6".toDouble

val number = "345a".toDouble //Error

val number = "345.6".toInt //Error	

s"foo\nbar"
raw"foo\nbar"

val c = 'A'

val t: Char = 116

val mybool = false

val unequal = (5 != 6)

val isLess = (5 < 6)

//Scala does not support automatic conversion of other types to Boolean. A non-null string does not evaluate as true, and the number zero does not equal false. Strings and numbers must be used in an explicit logical expression to return a Boolean type.

val info = (5, "Korben", true)

//access tuple element
info._1

val (age, name, married) = info

//Key-value paired tuple
val red = "red" -> "0xff0000"

val reversed = red._2 -> red._1

//Nested tuple
val tuple=((1,2),(("A","B"),("C",3)),4)

val k = (3,5)

val kReversed = k.swap

//The Any, AnyVal, and AnyRef types are the root of Scala’s type hierarchy.

//Any is the absolute root, and all other types descend from its two children, AnyVal and AnyRef. 

//The types that extend AnyVal are known as value types because they are the core values used to represent data.

//is a subtype of all Null types that exists to provide a special type for indicating that a given value or variable from the AnyRef type is not referencing an actual data instance in memory. 

//Nothing is a subtype of every other type (including user defined custom types) and exists to provide a compatible return type for operations that significantly affect a program’s flow.

//The type is similarly used in Scala Unit as the return type for functions or expressions that don’t return anything
```

***Expression***

An expression block is multiple expressions and / or statements collected together using curly braces ({ and }). There may be more than one expression in an expression block; however only the last line in the in the block determines the return value for the entire block.

```scala
val x = 5 * 20; val amount = x + 10

//Written as an expression block that returns a value used to in the final statement
val amount = { val x = 5 * 20; x + 10 }

val x = 10
val y = 20
val result = {x + y; x - y; x * y} //Result will be 200 as only the last line in the block determine the result
```

***Conditional Expressions***

```scala
if ( 47 % 3 > 0 ) println("Not a multiple of 3")


val x = 10; val y = 20; val max = if(x > y) x else y

val x = 10; val y = 20;
if(x > y)
{val z = 15; z + y}
else
{val p = 50; p - x}

var x = 20;
if( x == 10 ){
println("Value of X is 10");
} else if( x == 20 ){
println("Value of X is 20");
} else if( x == 30 ){
println("Value of X is 30");
} else {
println("Nothing matches");
}

val result = if ( false ) "what does this return?"

//The type of the result value in this example is unspecified so the compiler used type inference to determine the most appropriate type. Either a String or Unit could have been returned, so the compiler chose the root class Any. This is the one class common to both String (which extends AnyRef) and to Unit (which extends AnyVal).

if ((x == 0) || (y >= 10) && !(z < 6)) println ("good")

//match expression
val x = 10; val y = 20;
val max = x > y match {
    case true => x
    case false => y
}

val x = 10; val y = 20
val result = x + y match {
    case 30 => {val z = 10; z + 15}
    case 40 => println("second case match")
}

val day = "MON"
val beerprice = day match {
    case "MON" | "TUE" | "WED" | "THU" | "FRI" =>
        {println ("weekdays !"); 6}
    case "SAT" | "SUN" =>
        {println ("weekends !"); 10}
}

val message = "dog"
val status = message match {
    case "Ok" => 200
    case "Retry" => 300
    case "Transmit" => 400
    case other => {
        println(s"Couldn't parse $other")
        -1}
}

val message = "dog"
val status = message match {
    case "Ok" => 200
    case "Retry" => 300
    case "Transmit" => 400
    case _ => {
        println(s"Couldn't parse $message")
        -1}
}

//Note that the wildcard cannot be accessed on the right side of the arrow, unlike with value binding. To address this, the input to the match expression is used to create an informative statement println that does not appear for the case of value binding.

val code = ('h', 204, true)
code match {
    case (_, _, false) => 501
    case ('c', _, true) => 302
    case ('h', x, true) => x
    case (other, x ,true) => {println(s"Did not expext code $other");
    x}
}

val code = 204
code match {
    case x => x
}

val x = 20; val y = 10
val result = x+y match {
    case 30 if x > 5 && y <20=> {val z = 10; z +15}
	case 40 if y < 40 => println("second case match")
}

//Matching types with pattern variables

val x: Int = 12
val y: Any = x
y match {
    case x: String => s"The string is 'x'"
    case x: Double => f"The double to 2 decimal points is $x%.2f"
    case x: Float => f"The float to 2 decimal points is $x%.2f"
    case x: Long => s"The long is ${x}l"
    case x: Int => s"The int is ${x}i"
}
```

***Loops***

```scala
for (x <- 1 to 4){ println(s"Day $x:")}

for (x <- 2 until 5) { println(s"Day $x:") }

val result = for(x <- 1 to 4) yield {x + 3}
for (numbers <- result) print(numbers + ", ")

val myrange = 1 to 10
for (x <- myrange by 2) { .. }

val threes = for (i <- 1 to 20 if i % 3 == 0) yield i

val result = for(i <- 1 to 30 if i % 2 == 0 if i > 10 if i < 20) yield i

for (x <- 1 to 2) {for (y <- 1 to 3) print(s"($x,$y) ")}

val test = for(x <- 1 to 2; y<- 1 to 2) yield (x,y)

var x = 3
while (x > 0) {
    x -= 1; print (s"$x ")
}

val x = 0
do {
    println(s"Here I am, x = $x")
} while (x > 0)
```

***Breaks and continue***

```scala
import util.control.Breaks._

for(i <- 1 to 10){
    breakable{
        if(i % 2 == 0) break
        println(i)
    }
}
```

***Functions***

In Scala, functions are named, reusable expressions. They are usually parameterized and may return a value. They are usually a mix of both pure and impure functions.

```scala
def multiplier(x: Int, y: Int): Int = x * y
multiplier(6, 7)

def NumFunc(x: Int, y: Double): Double = {
    val z = x + 3;
    z / y;
}

//The return keyword can be used for an early function exit prior to the last line being executed and will specify a function’s return value.

def safeDivision(num1: Int, num2: Int): Double = {
    if(num2 == 0) return 0;
    num1.toDouble / num2;
}

//Returning multiple results through a tuple
def swap(x:String, y:String) = (y, x)
val (a,b) = swap("hello","world")
println(a, b)

//Simpler function definitions
//A function can be defined with a parameter list but without a return type. The function can still return a value through the last line in the expression block of the function; the Scala compiler will infer the type of this return value.
def multiplier(x: Int, y: Int) = x * y

//However, methods that have a return statement or are recursive must explicitly declare a return type.

//A function can also be defined without any input parameters and only the return type specified. The syntax for this is as follows:

def AddTwo: Int = {val x = 3; val y = 2; x + y}

def hi():String = "hi"

//Note that functions that have been declared without any parenthesis cannot be invoked with them, even if they are empty parenthesis.
def AddTwo:Int = {val x = 3; val y = 2; x + y}
AddTwo() // Error

//The simplest form of a function is one without any input parameters or any return type explicitly specified.

def greeting = "hi"
def AddTwo = {val x = 3; val y = 2; x + y}

//A procedure is a function that doesn’t have a return value. Any function that ends with a statement, such as a println() call, is also a procedure.

def DoNothing = println("hello")

def DoNothing() = {val z = 5}

def log(d: Double):Unit = println(d)

def DoNothing(x: Int):Unit = {x + 5}

//Methods
//A method is simply a function defined in a class and available from any instance of the class. The standard way to invoke methods in Scala is with the dot operator.

val s = "vacation.jpg"
val isJPEG = s.endsWith(".jpg")

val d = 65.642
d.round
d.floor

//Working with function parameters
//Using an expression block to invoke a function with a single parameter simplifies the calling of the function when the result returned from an expression block is to be used as the parameter of the function.
def NumFunc(x: Int): Int = x + 5
NumFunc {val z = 10; val y = 20; z + y}

//Named parameter

def greet(prefix: String, name: String) = s"$prefix $name"
val greeting1 = greet("Ms", "Brown")
greeting1: String = Ms Brown
val greeting2 = greet(name = "Brown", prefix = "Mr")
greeting2: String = Mr Brown

//Param with default values
def MyFunc(x: Int, y: Int = 3, z: Int = 5): Int = x + y + z

//Vararg Parameters
def sum(items: Int*): Int = {
    var total = 0
    for(i <- items) total += i
    total
}

//Parameter groups
def GetSum(x: Int)(y: Int, z:Int) = x + y + z
GetSum(3)(5,7)

//Recursive functions
def power(x: Int, n: Int): Long = {
    if(n >= 1) x * power(x, n-1) else 1
}
power(2, 8)

//Nested functions
def max(a: Int, b: Int, c: Int) = {
def max2(x: Int, y: Int) = if (x > y) x else y
max2(a, max2(b, c))
}
```

***First class functions***

A first-class function may be created in literal form; be stored in a container such as a value, variable, or data structure; be used as a parameter to another function or used as the return value from another function.

```scala
//Function types and values
def doubler(x: Int): Int = x * 2

//Function value
val myDouble: (Int) => Int = doubler

//Here we declare a function value myDouble and provide a function type for it ((Int) => Int) – i.e. a function that accepts a single Int parameter and returns a single Int.

def doDouble(x: Int): Int = x * 2
def addTen(x: Int): Int = x + 10
var myDouble: (Int) => Int = doDouble
myDouble = addTen

//A function value can also be defined with a function type that involves multiple parameters
def max(a: Int, b: Int) = if(a > b) a else b
val maximize: (Int, Int) => Int = max

//A function value can also be defined with a function type that does not have any input parameters
def message(): String = "Happy days"
val message:() => String = message
```

***Higher order functions***

A higher-order function is a function that has a value with a function type as an input parameter or return value.

```scala
def CheckMathOp(x: Int, y: Int, FuncToUse: (Int, Int) => Double) = {
    if((x == 0) || (y == 0))
    	x
    else
    	FuncToUse(x, y)
}

def DivideFunc(p: Int, q: Int): Double = p/q.toDouble

CheckMathOp(3, 5, DivideFunc)
```

***By-Name Parameters***

An alternate form of a function type parameter that can be accepted by a higher-order function is a by-name parameter. This parameter can take either a value or a function that returns a value.

```scala
def doubles(x: => Int): Int = {
    println("Now doubling " + x); x * 2
}

def MyFunc(i: Int): Int = i

doubles(5)
doubles(MyFunc(8))
```

***Function literals***

A function literal is a normal function without a name and are usually assigned to function values or variables. They are useful when the function only needs to be used exactly once, in which case providing a name for the function is unnecessary.

```scala
def doubler(x: Int): Int = x * 2

val myDouble: (Int) => Int = doubler

//If we use a function literal, we can combine the declaration of the function with its assignment to the value. In this case, we no longer need to explicitly declare the function type. The Scala interpreter will infer this for us.

val myDouble = (x:Int) => x * 2

val maximize = (a: Int, b: Int) => a + b

val maximize:(Int, Int) => Int = max

def CheckMathOp(x: Int, y: Int, FuncToUse: (Int, Int) => Double) = {
    if ((x == 0) || (y == 0)) x
    else FuncToUse(x, y)
}

CheckMathOp(3,5,(x: Int, y: Int) => x/y.toDouble)

CheckMathOp(3,5,(x, y) => x/y.toDouble)

def CheckMathOp(x: Int, y: Int)(FuncToUse: (Int, Int) => Double) = {
    if ((x == 0) || (y == 0)) x
    else FuncToUse(x, y)
}

CheckMathOp(3,5)((x, y) => x/y.toDouble)

//Placeholder syntax
val myDouble = (x: Int) => x * 2

val myDouble: (Int) => Int = _ * 2

val subtractTwo: (Int, Int) => Int = _ - _

CheckMathOp(3,5,(x, y) => x/y.toDouble)

CheckMathOp(3,5,_/_.toDouble)
```

***List***

```scala
val numbers: List[int] = List(1,2,3,4)

val colors = List("red", "green", "blue")

val numbers = List(32, 95, true)

val x = List.range(1,10)

val x = List.range(1,10,2)

val x = List(1 to 9)

val x = List(0 to 8 by 2)

val x = List.fill(3)("foo")

//Basic List methods

colors.head
colors.tail
colors(0)
colors.size
colors.contains("blue")

val l: List[Int] = List()
l == Nil //true

val m: List[String] = List("a")
m.head
m.tail == Nil //true

//Iterating using for, do-while and recursively
val numbers = List(32, 95, 24, 21, 17)
var total = 0; for(i <- numbers) {total += i}

val colors = List("red", "green", "blue")
for (c <- colors) { print(c + " ") }

while(i != Nil){ println(i.head + " "); i = i.tail }

//Nested List
val oddsAndEvens = List(List(1, 3, 5), List(2, 4, 6))
oddsAndEvens(0)
oddsAndEvens(0)(2)

val nestedLists = List(List(List(1,2,3), List(4,5,6)), List(List(7,8,9), List(0,1,2)))
nestedLists(1)(0)(2)

//List of tuples
val tupleList = List((1,2),(1,2),(1,2))
tupleList(0)._1

//Create List
val numbers = 1::2::3::Nil

val firstList = List(1,2,3)
val secondList = 4 :: firstList

//To append the 4 to the end of the list, we use the ::: operator instead for appending two lists
val secondList = firstList ::: List(4)

val words = List("Cat", "Donkey", "Mouse")
var wordsize = List[Int]()
for(elem <- words){
   wordsize = wordsize ::: List(elem.length) 
}

//foreach
colors.foreach((c:String) => print(c + " "))

colors.foreach( c => print(c + " "))

colors foreach println

//filter
val numbers = List(23, 8, 14, 21, 18)
numbers.filter((i: Int) => (i < 20))
numbers.filter( _ < 20)
numbers.filter((i: Int) => (i > 10) && (i % 2 > 0))

//partition (return tuple)
val numbers = List(23, 8, 14, 21, 18)
val results = numbers.partition(_ < 20)

//groupBy
val x = List(15, 10, 5, 8, 20, 12)
val y = x.groupBy(_ > 10)

val x = List("mouse", "cat", "monster", "curry")
val y = x.groupBy(_.charAt(0))

//sortBy
val numbers = List(23, 8, 14, 21, 18)
numbers.sortBy(_.toInt)
numbers.sortBy(identity)

//The toInt is an redundant method call which simply returns the exact number as writing numbers.sortBy( _ ) results in a missing parameter type error.

val words = List("horse", "cat", "elephant", "monkey")
words.sortBy(_.size)

words.sortBy(_.charAt(0))(Ordering[Char].reverse)
numbers.sortBy(identity)(Ordering[Int].reverse)

//Mapping
val colors = List("red", "green", "blue")
colors.map( (c: String) => c.size )

val sizes = colors.map(_.size)

val oddsAndEvens = List(List(1, 3, 5), List(2, 4, 6))
oddsAndEvens.flatMap(identity)

val people = "jack,peter,ryan,wendy"
people.split(",")

val foodItems = List("milk,coffee", "apple,banana,orange")
foodItems.flatMap((c: String) => c.split(","))

val numbers = List(24, 17, 32)
numbers.exists((i:Int) => i < 18)
numbers.exists(_ < 18)
```

***foldLeft***

```scala
val numbers = List(2, 8, 7, 3)
numbers.foldLeft(5)((a,b) => {println(s"a: $a; b: $b"); a+b})
numbers.foldLeft(5)( _ + _ )

//For the first invocation of foldLeft(), the initial value 5 is passed to a and the first element 3 in the list is passed to b. The function is invoked and the result 7 is passed to parameter a for the next invocation, with b now holding the next element in the list, 8. This process is repeated until the end of the list is reached. In this example, the initial value is set to some arbitrary value, but it should be set to 0 if we wish to obtain the sum of all the elements in the list.

val numbers = List("3", "8", "7", "2")
numbers.foldLeft(0)((a: Int, b: String) => a + b.toInt)
numbers.foldLeft(0)(_ + _.toInt)

//Here, the Scala compiler is able to infer that the first parameter type is Int (because that is the type of the initial value 0) and the second parameter type will be the List type.
```

***foldRight***

```scala
//foldRight()works in exactly the same fashion as foldLeft(), except that it operates from right to left on the list. For a reduction function with two parameters a and b, the first invocation of the function will assign the right most element (last element) in the list to a and the initial value to b. The result of the function is then stored in b, and the next element in sequence from the right in the list is assigned to a for the next invocation of the reduction function. This continues until the start of the list is reached.

val numbers = List(2, 8, 7, 3)
numbers.foldRight(5)((a,b) => {println(s"a: $a; b: $b"); a+b})
numbers.foldRight(5)( _ + _ )

val sentence = List("Mary", "had", "a", "little", "lamb")
    sentence.foldLeft("start")((a,b) => {
    println("[a:" + a + "][b:"+ b + "]");
    a + b
})

val sentence = List("Mary", "had", "a", "little", "lamb")
    sentence.foldRight("start")((a,b) => {
    println("[a:" + a + "][b:"+ b + "]");
    a + b
})

val numbers = List("3", "8", "7", "2")
numbers.foldRight(0)( (a: String, b: Int) => a.toInt + b)
```

***Iterator***

```scala
val it = List(1,2,3,4,5).iterator
while(it.hasNext)
println(it.next())

for (elem <- it) print(elem + " ")

it foreach println

//There's an important difference between the foreach method on iterators and the same method on traversable collections: When called to an iterator, foreach will leave the iterator at its end when it is done. So calling next again on the same iterator will fail with a NoSuchElementException. By contrast, when called on a collection, foreach leaves the number of elements in the collection unchanged

val it = Iterator("a", "number", "of", "words")
val result = it.map(_.length)
result foreach println
```

***Map***

```scala
//A Map (also known as a hashmap, dictionary, or associative array in other languages) is an immutable key-value collection. Elements stored in a Map consist of key-value pairs, and each value can be retrieved using its associated key. All keys are unique; there cannot be duplicate keys in a Map.

//Map is also a subclass of Iterable and therefore supports the same operations as List does.

var colorMap = Map("red" -> 5, "green" -> 6, "blue" -> 7)

//Duplication keys will be dropped out
val colors = Map("Red" -> 0, "Green" -> 1, "Red" -> 2)

//Retrieval operations
colorMap("red")
colorMap("green")
colorMap get "red"
colorMap get "dog"
colorMap.getOrElse("purple" , 35)
colorMap.contains("white")

//Additions, updates and removals
val newMap = colorMap + ("orange" -> 20, "yellow" -> 25)

//Addition between 2 maps
newMap ++ colorMap

nextMap - ("green", "blue")

//Sub collections and iteration
for (pairs <- colorMap) print(pairs + " ")

for((k, v) <- colorMap) println(s"key: $k, value: $v")
colorMap foreach (x => println(x._1 + "-->" + x._2))
colorMap foreach { case (key,value) => println (key + "-->" + value)}

for (mykeys <- colorMap.keys) { print(mykeys + " ") }
for (mykeys <- colorMap.values) { print(mykeys + " ") }

colorMap.keys.foreach{ i => print( "Key = " + i ); println(" Value = " + colorMap(i))}
```

***Transformations***

```scala
val colorMap = Map("red" -> 5, "green" -> 10, "blue" -> 15)

colorMap.map(t => t._1)
colorMap.map(t => t._2)
colorMap.filterKeys(_.size > 3)
colorMap.mapValues(_ + 100)
val newMap = colorMap.map((t: (String, Int)) => (t._1 + "wonder", t._2 + 10))
```

***Set***

```
val unique = Set(10, 20, 30, 20, 10)
unique(10)
unique(20)

val s1 = Set(1, 2)

val s2 = s1 + 3

val s3 = s2 + (4, 5)

val s4 = s3 ++ List(6, 7)

val low = 1 to 5 toSet
low.toList

val medium = (3 to 7).toSet

val uniq = low.union(medium)

val i = low.intersect(medium)

val diff = low.diff(medium)

val diff = medium.diff(low)
```

***Array***

```scala
var x = new Array[String](3)

var x = new Array[String](0)
//Here we have any array of size 0, but this is not considered null
x.length

val m: Array[String] = Array("a")
m.head
m.tail

import Array._
var myList1 = Array(1, 2, 3)
var myList2 = Array(4, 5, 6)
var myList3 = concat(myList1, myList2)

import Array._
var my2darray = ofDim[Int](3,3)
    for (i <- 0 to 2) {
    for ( j <- 0 to 2) {
    my2darray(i)(j) = j+3;
    }
}

for (i <- 0 to 2) {
    my2darray(i) foreach print
    println
}
```

***Tuple***

```scala
val t = (4,3,2,1)

t.productIterator
res1: Iterator[Any] = non-empty iterator
t.productIterator.foreach{ i => println("Value = " + i )}

t.productIterator.toList

val newList = t.productIterator.toList.map(x => x.asInstanceOf[Int])

val t = (1, "dog", false)
t.productIterator.foreach {
    case s: String => println("nice " + s)
    case i: Int => println("int: " + (i+20))
    case b: Boolean => println (!b)
}
```

***String***

```scala
for (c <- "hello") yield (c+2).toChar

"hello" foreach println
```



