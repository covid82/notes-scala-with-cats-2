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

  implicit val booleanCAndSemigroup = new BooleanAndSemigroup
  class BooleanAndSemigroup extends Semigroup[Boolean] {
    def combine(x: Boolean, y: Boolean): Boolean = x && y
  }

  implicit val booleanAndSemigroup = new BooleanAndSemigroup
  class BooleanAndSemigroup extends Semigroup[Boolean] {
    def combine(x: Boolean, y: Boolean): Boolean = x || y
  }

  implicit val booleanXorSemigroup = new BooleanXorSemigroup
  class BooleanXorSemigroup extends Semigroup[Boolean] {
    def combine(x: Boolean, y: Boolean): Boolean = ( !x && !y ) || ( x && y )
  }

  implicit val booleanXnorSemigroup = new BooleanXnorSemigroup
  class BooleanXnorSemigroup extends Semigroup[Boolean] {
    def combine(x: Boolean, y: Boolean): Boolean = ( !x && y ) || ( x && !y )
  }

  implicit def setOrSemigroup[A] = new SetOrSemigroup[A]
  class SetAndSemigroup[A] extends Semigroup[A] {
    def combine(xs: Set[A], ys: Set[A]): Set[A] = xs union ys
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
```scala
trait Monoid[A] extends Semigroup[A] {
  def empty: A
}
```
### Instances
```scala
object Monoid {
  class StringMonoid extends Monoid[String] with StringSemigroup {
    def empty: String = ""
  }
  class BooleanAndMonoid extends Monoid[Boolean] with BooleanAndSemigroup {
    def empty: Boolean = true
  }
  class BooleanOrMonoid extends Monoid[Boolean] with BooleanOrSemigroup {
    def empty: Boolean = false
  }
  class SetOrMonoid[A] extends Monoid[Set[A]] with SetOrSemigroup[A] {
    def empty: Boolean = Set()
  }
}
```
### Laws
```scala
trait MonoidLaws[A : Monoid] extends SemigroupLaws[A] {
  def identityLaw(a: A): Boolean =
    a |+| Monoid[A].empty == Monoid[A].empty |+| a == a
}
```
