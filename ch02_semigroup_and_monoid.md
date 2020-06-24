### Type class
```scala
trait Semigroup[A] {
  @Op("|+|) def combine(x: A, y: A): A
}
```scala
### Instances
```
object Semigroup {

  implicit val stringSemigroup = new StringSemigroup
  class StringSemigroup extends Semigroup[String] {
    def combine(x: String, y: String): String = x + y
  }

  implicit val intAdditionSemigroup = new IntAdditionSemigroup
  class IntAdditionSemigroup extends Semigroup[Int] {
    def combine(x: Int, y: Int): Int = x + y
  }

  implicit val intMultiplicationSemigroup = new IntMultiplicationSemigroup
  class IntMultiplicationSemigroup extends Semigroup[Int] {
    def combine(x: Int, y: Int): Int = x * y
  }

  implicit val booleanConjunctionSemigroup = new BooleanConjunctionSemigroup
  class BooleanConjunctionSemigroup extends Semigroup[Boolean] {
    def combine(x: Boolean, y: Boolean): Boolean = x && y
  }

  implicit val booleanDisjunctionSemigroup = new BooleanDisjunctionSemigroup
  class BooleanDisjunctionSemigroup extends Semigroup[Boolean] {
    def combine(x: Boolean, y: Boolean): Boolean = x || y
  }
}
```

### Laws
- Associativity law:
  ```scala
  (a |+| b) |+| c = a |+| (b |+| c)
  ```
