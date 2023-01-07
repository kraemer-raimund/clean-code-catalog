# Naming Interfaces and Implementations

## Summary

&cross; DO NOT add an `Impl` suffix to implementations or an `I` prefix to interfaces. [[1](https://martinfowler.com/bliki/InterfaceImplementationPair.html)][[2](https://octoperf.com/blog/2016/10/27/impl-classes-are-evil/)]

&check; DO name interfaces for what they are. [[1](https://stackoverflow.com/a/2814831/3726133)]

&check; DO name implementations more specific than interfaces. [[1](https://web.archive.org/web/20130331071928/http://isagoksu.com/2009/development/java/naming-the-java-implementation-classes)][[2](https://stackoverflow.com/a/2814831/3726133)]

## Examples
- `RacingCar implements Car`, not ~~`CarImpl implements Car`~~
- `DefaultLogger implements Logger` (if a default implementation should be provided and no more appropriate name can be found; better `ConsoleLogger`, `FileLogger`, etc.)
- In the standard library, `List` is an interface, with `ArrayList` etc. being one of the implementations.
- [Spring Framework uses](https://github.com/spring-projects/spring-framework/tree/v6.0.3/spring-core/src/main/java/org/springframework/core/serializer) `Serializer` and `DefaultSerializer`, etc.

## Reasoning

People arguing for the `Impl` suffix often do so [stating that it is common practice](https://fallacyinlogic.com/appeal-to-tradition-fallacy-definition-and-examples/). `Impl` suffix and `I` prefix are commonly [discussed as the 2 options in the decision](https://www.logicallyfallacious.com/logicalfallacies/False-Dilemma).

We don't add `Impl` to all concrete classes to show that they are not interfaces. It is only ever used in cases where there is only a **single implementation**; we wouldn't add suffixes `Impl`, `Impl2` and so on if there were multiple implementations.

There are good reasons to use an interface even with only one implementation. Typically it is because of [dependency injection](https://stackoverflow.com/a/2815440/3726133). When dependency *injection* is used, the thinking is often implementation-first, with the interface being introduced as a syntactical mechanism if the dependency injection framework requires it, or in an attempt to achieve loose coupling. But many programmers misunderstand [dependency *inversion*](https://stackoverflow.com/a/46745172/3726133). As a result, the dependency is not *inverted*. The consumer of the dependency still relies on the concrete implementation, even if it only accesses it via the interface. It is [indirection without abstraction](https://www.silasreinagel.com/blog/2018/10/30/indirection-is-not-abstraction/). By thinking about the abstract interface first, the implementation can be sufficiently decoupled so that a meaningful name becomes more obvious. Dependency inversion as a principle, not dependency injection as a mechanism, should be the reason for creating an interface in the absence of polymorphism.

Imagine `ArrayList` didn't exist, so you create your own. To "decouple" the consumer from the class, you introduce an interface `ArrayList` and name the implementation `ArrayListImpl`, with the exact same public interface as in the concrete class. No decoupling has happened here. You're still dependant on the `ArrayListImpl`, you just don't directly reference it. What we want to do intead is to depend on the more abstract interface. In this example, `List` or even better `Iterable`, depending on the specific usage, are much smaller in terms of public interface than the concrete implementation.
