## Semigroup

### Type class
```scala
@typeclass trait Semigroup[A] {
  @Op("|+|) def combine(x: A, y: A): A
}
```

### Instances
```scala
object Semigroup {

  // string
  implicit val stringSemigroup = new StringSemigroup
  class StringSemigroup extends Semigroup[String] {
    def combine(x: String, y: String): String = x + y
  }

  // list
  implicit def listSemigroup[A] = new ListSemigroup[A]
  class ListSemigroup[A] extends Semigroup[List[A]] {
    def combine(xs: List[A], ys: List[A]): String = xs ++ ys
  }

  // int
  implicit val intAdditionSemigroup = new IntAdditionSemigroup
  class IntAdditionSemigroup extends Semigroup[Int] {
    def combine(x: Int, y: Int): Int = x + y
  }

  implicit val intMultiplicationSemigroup = new IntMultiplicationSemigroup
  class IntMultiplicationSemigroup extends Semigroup[Int] {
    def combine(x: Int, y: Int): Int = x * y
  }

  // boolean
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

  // set
  implicit def setUnionSemigroup[A] = new SetUnionSemigroup[A]
  class SetUnionSemigroup[A] extends Semigroup[A] {
    def combine(xs: Set[A], ys: Set[A]): Set[A] = xs union ys
  }

  implicit def setSymDiffSemigroup[A] = new SetSymDiffSemigroup[A]
  class SetSymDiffSemigroup[A] extends Semigroup[A] {
    def combine(xs: Set[A], ys: Set[A]): Set[A] = (xs diff ys) union (ys diff xs)
  }
}
```

### Laws
```scala
trait SemigroupLaws[A: Monoid] {
  def associativity(x: A, y: A, z: A): Boolean =
    ((x |+| y) |+| z) == (x |+| (y |+| z))
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
  // string
  class StringMonoid extends Monoid[String] with StringSemigroup {
    def empty: String = ""
  }

  // list
  class ListMonoid[A] extends Monoid[A] with ListSemigroup[A] {
    def empty: List[A] = List.empty[A]
  }

  // boolean
  class BooleanAndMonoid extends Monoid[Boolean] with BooleanAndSemigroup {
    def empty: Boolean = true
  }
  class BooleanOrMonoid extends Monoid[Boolean] with BooleanOrSemigroup {
    def empty: Boolean = false
  }

  // set
  class SetUnionMonoid[A] extends Monoid[Set[A]] with SetUnionSemigroup[A] {
    def empty: Boolean = Set()
  }

  class SetSymDiffMonoid[A] extends Monoid[Set[A]] with SetSymDiffSemigroup[A] {
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
