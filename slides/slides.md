## Who am I?

- [@47er](https://twitter.com/47er), I'm working in [@47deg](https://twitter.com/47deg)
- Lorem ipsum dolor sit amet, consectetur adipiscing elit

---

## Agenda

1. An introduction to __Bow__
2. __Top 5__ Swift features FP programmers love
4. Lorem ipsum dolor sit amet, consectetur adipiscing elit
4. __Fx__. sed do eiusmod tempor incididunt ut labore et dolore magna aliqua

---

An introduction to __Bow__

---

Bow started as lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua

<img src="img/47-brand-red.png" width="200">

---

...Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat

![Kategory](img/47-brand-white.png)

---

Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur

![Bow](img/bow-brand-color.png)

---

Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum...

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

__Lorem__ ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.

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

| Type class | Combinators | **List** | **Either** |
| --- | --- | --- | --- |
| Functor | map, lift | ✓ | ✓ |
| Applicative | pure, ap | ✓ | ✓ |
| ApplicativeError | raiseError, catch | ✕ | ✓ |
| Monad | flatMap, flatten | ✓ | ✓ |
| MonadError | ensure, rethrow | ✕ | ✓ |
| MonadDefer | delay, suspend | ✕ | ✕ |
| Async | async | ✕ | ✕ |
| Effect | runAsync | ✕ | ✕ |

---

__Bow__ is modular

<p>
  <img alt="bow-feature-icon-tertiary" src="img/bow-feature-icon-primary.svg" style='width:100%; height:auto;max-width:80px'>
</p>

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

## Nullable Types

---

## Nullable Types

<img alt="Feature icon primary" src="img/bow-feature-icon-primary.svg" style='width:100%; height:auto;max-width:240px'>


[http://natpryce.com/articles/000818.html](http://natpryce.com/articles/000818.html)

---

## Nullable Types

<img alt="Feature icon secondary" src="img/bow-feature-icon-secondary.svg" style='width:100%; height:auto;max-width:240px'>


[http://natpryce.com/articles/000818.html](http://natpryce.com/articles/000818.html)

---

## Nullable Types

<img alt="Feature icon tertiary" src="img/bow-feature-icon-tertiary.svg" style='width:100%; height:auto;max-width:240px'>

[http://natpryce.com/articles/000818.html](http://natpryce.com/articles/000818.html)

---

## Nullable Types

<img alt="Feature icon quarter" src="img/bow-feature-icon-quarter.svg" style='width:100%; height:auto;max-width:240px'>

[http://natpryce.com/articles/000818.html](http://natpryce.com/articles/000818.html)

---

## Nullable Types

```swift
func divide<F: ErrorSuccessRepresentable>(x: Int, y: Int) -> F<DivideError, Int> {
   guard y != 0 else { return .failure(.divisionByZero)
   return .success(x / y)
}
```

---

## Lorem ipsum

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.

Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

---

## Credits

Bow is inspired by great libraries that have proven useful to the FP community:

- [Cats](https://typelevel.org/cats/)
- [Scalaz](https://github.com/scalaz/scalaz)
- [Mu](http://higherkindness.io/mu/)
- [Monocle](http://julien-truffaut.github.io/Monocle/)
- [Arrow](https://github.com/arrow-kt/arrow)

---

## Join us!

|        |                                                 |
|--------|-------------------------------------------------|
| Github | https://github.com/bow-swift             |
| Gitter | https://gitter.im/bow-swift/bow               |

We are beginner friendly and provide 1:1 mentoring for both users & new contributors!
+12 Contributors and growing!

---

## Thanks!

### Thanks to everyone that makes Bow possible!

<img src="img/47deg-logo.png" height="200">
<img src="img/bow-logo.png" height="200">
