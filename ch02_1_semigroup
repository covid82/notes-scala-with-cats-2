##### Create `io.covid82.fps.Semigroup` in `src/main/scala`:
```scala
package io.covid82.fps

import simulacrum.{op, typeclass}

@typeclass trait Semigroup[A] {
  @op("|+|") def combine(x: A, y: A): A
}

object Semigroup {

  implicit val booleanAndSemigroup: Semigroup[Boolean] = new BooleanAndSemigroup

  class BooleanAndSemigroup extends Semigroup[Boolean] {
    override def combine(x: Boolean, y: Boolean): Boolean = x && y
  }
}

trait SemigroupLaws {

  import Semigroup.ops._

  def associativityLaw[A: Semigroup](x: A, y: A, z: A): Boolean =
    ((x |+| y) |+| z) == (x |+| (y |+| z))
}
```

##### Create `io.covid82.fps.SemigroupSpec` in `src/test/scala`:
```scala
package io.covid82.fps

import org.scalacheck.Arbitrary
import org.scalatest.matchers.must
import org.scalatest.propspec.AnyPropSpec
import org.scalatestplus.scalacheck.ScalaCheckPropertyChecks

class SemigroupSpec[A](semigroup: Semigroup[A], arbitrary: Arbitrary[A])
  extends AnyPropSpec
    with ScalaCheckPropertyChecks
    with must.Matchers
    with SemigroupLaws {

  implicit val sgp: Semigroup[A] = semigroup
  implicit val arb: Arbitrary[A] = arbitrary

  property("semigroup combine should be associative") {
    forAll { (x: A, y: A, z: A) =>
      assert(associativityLaw(x, y, z))
    }
  }
}

class BooleanAndSemigroupSpec extends SemigroupSpec[Boolean](Semigroup.booleanAndSemigroup, Arbitrary.arbBool)
```
