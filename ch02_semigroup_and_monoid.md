## Semigroup

### Type class
```scala
trait Semigroup[A] {
  @Op("|+|) def combine(x: A, y: A): A
}
```

### Instances
```scala
object Semigroup {

  implicit val stringSemigroup = new StringSemigroup
  class StringSemigroup extends Semigroup[String] {
    def combine(x: String, y: String): String = x + y
  }

  implicit def listSemigroup[A] = new ListSemigroup[A]
  class ListSemigroup[A] extends Semigroup[List[A]] {
    def combine(xs: List[A], ys: List[A]): String = xs ++ ys
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
```scala
trait SemigroupLaws[A: Monoid] {
  def associativity(x: A, y: A, z: A): Boolean =
    (x |+| y) |+| z = x |+| (y |+| z)
}
```

## Monoid
### Type class
```
trait Monoid[A] extends Semigroup[A] {
  def empty: A
}
```
### Laws
```
trait MonoidLaws[A : Monoid] extends SemigroupLaws[A] {
  def identityLaw(a: A): Boolean =
    a |+| Monoid[A].empty == Monoid[A].empty |+| a == a
}
```
