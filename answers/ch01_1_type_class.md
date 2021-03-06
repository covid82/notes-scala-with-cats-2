### Type class

In cats a type class is represented by a trait with at least one type parameter

There are three components of type class pattern:
- type class:
    ```scala
    trait Printer[A] {
      def print(value: A): String
    }
    ```
- type class instance:
    ```scala
    object PrinterInstances {
      implicit val intPrinter: Printer[Int] = new Printer[Int] {
        def print(value: Int): if (value != null) value.toString else ""
      }
    }
    ```
- type class interface:
  - interface object:
    ```scala
    object Printer {
      def print[A](value: A)(implicit printer: Printer[A]): String = printer.print(value)
    }
    ```
  - interface syntax:
    ```scala
    object PrinterSyntax {
      implicit class PrinterOps[A](value: A) {
        def print(implicit printer: Printer[A]): String = printer.print(value)
      }
    }
    ```
### Imports
```scala
// type class
import cats.Show

// instance
import cats.instances.int._

// syntax
import cats.syntax.show._
```
Import all:
```scala
import cats._
import cats.implicits._
```

### Variance
```
trait Shape
trait Circle extends Shape
```
- Covariant:
  ```scala  
  trait List[+A] {
    def head: Option[A]
    def tail: List[A]
  }
  
  val circles: List[Circle] = ???
  val shapes: List[Shape] = circles
  ```
- Contravariant:
  ```scala
  trait Printer[-A] {
    def print(value: A): String
  }
  
  val shapePrinter: Printer[Shape] = ???
  val circlePrinter: Printer[Circle] = shapePrinter
  ```
- Invariant:
  ```scala
  trait MutableList[A] {
    def add(value: A): Unit
    def get: A
  }
  ```
### Basic Cats Type Classes
- `Show`
- `Eq`
