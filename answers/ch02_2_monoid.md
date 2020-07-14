- Create `io.covid82.fps.Monoid` in `src/main/scala`:
  ```
  package io.covid82.fps

  import io.covid82.fps.Semigroup.BooleanAndSemigroup
  import simulacrum.typeclass

  @typeclass trait Monoid[A] extends Semigroup[A] {
    def empty: A
  }

  object Monoid {
    implicit val booleanAndMonoid: Monoid[Boolean] = new BooleanAndMonoid
    class BooleanAndMonoid extends BooleanAndSemigroup with Monoid[Boolean] {
      override def empty: Boolean = true
    }
  }

  trait MonoidLaws extends SemigroupLaws {

    import Monoid.ops._

    def leftIdentityLaw[A: Monoid](x: A): Boolean =
      (Monoid[A].empty |+| x) == x

    def rightIdentityLaw[A: Monoid](x: A): Boolean =
      (x |+| Monoid[A].empty) == x
  }
  ```
- Create `io.covid82.fps.SemigroupSpec` in `src/test/scala`:
  ```
  package io.covid82.fps

  import org.scalacheck.Arbitrary
  import org.scalatest.matchers.must
  import org.scalatest.propspec.AnyPropSpec
  import org.scalatestplus.scalacheck.ScalaCheckPropertyChecks

  class MonoidSpec[A](monoid: Monoid[A], arbitrary: Arbitrary[A])
    extends AnyPropSpec
      with ScalaCheckPropertyChecks
      with must.Matchers
      with MonoidLaws {

    implicit val mon: Monoid[A] = monoid
    implicit val arb: Arbitrary[A] = arbitrary

    property("monoid combine must be associative") {
      forAll { (x: A, y: A, z: A) =>
        associativityLaw(x, y, z)
      }
    }

    property("monoid must satisfy left identity law") {
      forAll { x: A =>
        leftIdentityLaw(x)
      }
    }

    property("monoid must satisfy right identity law") {
      forAll { x: A =>
        rightIdentityLaw(x)
      }
    }
  }

  class BooleanAndMonoid extends MonoidSpec[Boolean](Monoid.booleanAndMonoid, Arbitrary.arbBool)
  ```