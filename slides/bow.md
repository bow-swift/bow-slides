## Who am I?

- [@raulraja](https://twitter.com/raulraja), Co-Founder and CTO [@47deg](https://twitter.com/47deg)
- Typed FP advocate (for all languages)

---

## Agenda

1. An introduction to __ΛRROW__
2. __Top 5__ Kotlin features FP programmers love
4. The __Kotlin Suspension__ system
4. __Fx__. A solution for building typed FP programs in Swift

---

An introduction to __ΛRROW__

---

ΛRROW started as learning exercise in the spanish Android Community Slack

![inline](img/47-brand-red.png)

---

...at the time it was called KΛTEGORY and we had the coolest logo ever!

![Kategory](img/47-brand-white.png)

---

The name was cool but the community was more important

![Mario & Raul](img/47-logo.png)

---

ΛRROW = KΛTEGORY + Funktionale

![Arrow](img/bow-brand-color.png)

---

ΛRROW includes popular FP data types...

|                |                                                      |
|----------------|------------------------------------------------------|
| Error Handling | `Option`,`Try`, `Validated`, `Either`, `Ior`         |
| Collections    | `ListK`, `SequenceK`, `MapK`, `SetK`             |
| RWS            | `Reader`, `Writer`, `State`                          |
| Transformers   | `ReaderT`, `WriterT`, `OptionT`, `StateT`, `EitherT` |
| Evaluation     | `Eval`, `Trampoline`, `Free`, `FunctionN`            |
| Effects        | `IO`, `Free`, `ObservableK`                         |
| Optics         | `Lens`, `Prism`, `Iso`,...                           |
| Recursion      | `Fix`, `Mu`, `Nu`,...                                |
| Others         | `Coproduct`, `Coreader`, `Const`, ...                |

---

Kotlin lacks higher kinded types and real type classes

Emulating __higher kinded types__ is based on `defunctionalization`
[__Lightweight higher-kinded polymorphism__](https://www.cl.cam.ac.uk/~jdy22/papers/lightweight-higher-kinded-polymorphism.pdf)
by Jeremy Yallop and Leo White

```diff
+ @higherkind
+ class Option<A> : OptionOf<A>
- class ForOption private constructor() { companion object }
- typealias OptionOf<A> = ΛRROW.Kind<ForOption, A>
- inline fun <A> OptionOf<A>.fix(): Option<A> =
-   this as Option<A>
```

---

Once we have a `Kind` representation we can provide `extensions`

```swift
func allIntsEqual(_ array: [Int]) -> Bool {
    guard let first = array.first else { return false }
    return array.reduce(true) { partial, next in partial && next == first }
}

func allStringsEqual(_ array: [String]) -> Bool {
    guard let first = array.first else { return false }
    return array.reduce(true) { partial, next in partial && next == first }
}
```

---

This will export all extensions functions declared in `Functor` into `IO`

```swift
enum DivideError: Error {
    case divisionByZero
}

func divideEither(x: Int, y: Int) -> Either<DivideError, Int> {
    guard y != 0 else { return .left(.divisionByZero) }
    return .right(x / y)
}

func divideValidated(x: Int, y: Int) -> Validated<DivideError, Int> {
    guard y != 0 else { return .invalid(.divisionByZero) }
    return .valid(x / y)
}
```

---

... and type classes.

| Type class | Combinator |
| --- | --- |
| Semigroup | combine |
| Monoid | empty |
| Functor | map, lift |
| Foldable | foldLeft, foldRight |
| Traverse | traverse, sequence |
| Applicative | just, ap |
| ApplicativeError | raiseError, catch |
| Monad | flatMap, flatten |
| MonadError | ensure, rethrow |
| MonadDefer | delay, suspend |
| Async | async |
| Effect | runAsync |

---

| arrow-core |
| --- |
| ![Core type classes](css/images/core-typeclasses.png) |

---

| arrow-effects |
| --- |
| ![Efects Type Classes](css/images/effects-typeclasses.png) |

---

Data types may provide extensions for type classes based on capabilities:

| Type class | Combinators | **List** |
| --- | --- | --- |
| Functor | map, lift | ✓ |
| Applicative | just, ap | ✓ |
| ApplicativeError | raiseError, catch | ✕ |
| Monad | flatMap, flatten | ✓ |
| MonadError | ensure, rethrow | ✕ |
| MonadDefer | delay, suspend | ✕ |
| Async | async | ✕ |
| Effect | runAsync | ✕ |

---

Data types may provide extensions for type classes based on capabilities:

| Type class | Combinators | **List** | **Either** | **Deferred** | **IO** |
| --- | --- | --- | --- | --- | --- |
| Functor | map, lift | ✓ | ✓ | ✓ | ✓ |
| Applicative | pure, ap | ✓ | ✓ | ✓ | ✓ |
| ApplicativeError | raiseError, catch | ✕ | ✓ | ✓ | ✓ |
| Monad | flatMap, flatten | ✓ | ✓ | ✓ | ✓ |
| MonadError | ensure, rethrow | ✕ | ✓ | ✓ | ✓ |
| MonadDefer | delay, suspend | ✕ | ✕ | ✓ | ✓ |
| Async | async | ✕ | ✕ | ✓ | ✓ |
| Effect | runAsync | ✕ | ✕ | ✓ | ✓ |

---

ΛRROW is modular ![Android waving](css/images/android-waving.png)

| Module            | Contents                                                              |
|-------------------|-----------------------------------------------------------------------|
| typeclasses       | `Semigroup`, `Monoid`, `Functor`, `Applicative`, `Monad`...                      |
| core/data         | `Option`, `Try`, `Either`, `Validated`...                                     |
| effects           | `Async`, `MonadDefer`, `Effect`, `IO`...                                                                    |
| effects-rx2       | `ObservableK`, `FlowableK`, `MaybeK`, `SingleK`                                                          |
| effects-coroutines       | `DeferredK`                                                       |
| mtl               | `MonadReader`, `MonadState`, `MonadFilter`,...                              |
| free              | `Free`, `FreeApplicative`, `Trampoline`, ...                                |
| recursion-schemes | `Fix`, `Mu`, `Nu`                                                                     |
| optics            | `Prism`, `Iso`, `Lens`, ...                                                 |
| meta              | `@higherkind`, `@deriving`, `@extension`, `@optics` |

---

__Top 5__

Features Swift offers to FP

---

## 5

`?` Nullable Types

---

`?` Nullable Types

![inline](css/images/kotlin-types.png)

[http://natpryce.com/articles/000818.html](http://natpryce.com/articles/000818.html)



---

`?` Nullable Types

```swift
func divide<F: ErrorSuccessRepresentable>(x: Int, y: Int) -> F<DivideError, Int> {
   guard y != 0 else { return .failure(.divisionByZero)
   return .success(x / y)
}
}
```

---

KEEP-87

The ΛRROW team plans to submit this proposal at the end of Q1 2019 once it's solid and it has properly addressed feedback from the community and the jetbrains compiler team.

---

Credits

ΛRROW is inspired by great libraries that have proven useful to the FP community:

- [Cats](https://typelevel.org/cats/)
- [Scalaz](https://github.com/scalaz/scalaz)
- [Mu](http://higherkindness.io/mu/)
- [Monocle](http://julien-truffaut.github.io/Monocle/)
- [Funktionale](https://github.com/MarioAriasC/funKTionale)

---

## Join us!

|        |                                                 |
|--------|-------------------------------------------------|
| Github | https://github.com/arrow-kt             |
| Slack  | https://kotlinlang.slack.com/messages/C5UPMM0A0 |
| Gitter | https://gitter.im/arrow-kt/Lobby               |

We are beginner friendly and provide 1:1 mentoring for both users & new contributors!
+110 Contributors and growing!

---

## Thanks!

### Thanks to everyone that makes ΛRROW possible!

![47 Degrees](css/images/47deg-logo.png)  ![Kotlin](css/images/kotlin.png)
