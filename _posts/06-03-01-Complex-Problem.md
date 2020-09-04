---
isChild: true
anchor:  complex_problem
title: 复杂的问题
---

## 复杂的问题 {#complex_problem_title}

如果你曾经了解过依赖注入，那么你可能见过 *"控制反转"(Inversion of Control)* 或者 *"依赖反转准则"(Dependency Inversion Principle)*这种说法。这些是依赖注入能解决的更复杂的问题。

### 控制反转

顾名思义，一个系统通过组织控制和对象的完全分离来实现"控制反转"。对于依赖注入，这就意味着通过在系统的其他地方控制和实例化依赖对象，从而实现了解耦。

一些 PHP 框架很早以前就已经实现控制反转了，但是问题是，我们应该反转哪部分以及到什么程度？比如， MVC 框架通常会提供超类或者基本的控制器类以便其他控制器可以通过继承来获得相应的依赖。这就是控制反转的例子，但是这种方法是直接移除了依赖而不是减轻了依赖。

依赖注入允许我们通过按需注入的方式更加优雅地解决这个问题，完全不需要任何耦合。

### 依赖反转准则

依赖反转准则是面向对象设计准则 S.O.L.I.D 中的 "D" ,倡导 *"依赖于抽象而不是具体"*。简单来说就是依赖应该是接口/约定或者抽象类，而不是具体的实现。我们能很容易重构前面的例子，使之遵循这个准则

### 面向对象五大原则（S.O.L.I.D）

#### 单一替换原则

The Single Responsibility Principle is about actors and high-level architecture. It states that “A class should have
only one reason to change.” This means that every class should _only_ have responsibility over a single part of the
functionality provided by the software. The largest benefit of this approach is that it enables improved code
_reusability_. By designing our class to do just one thing, we can use (or re-use) it in any other program without
changing it.

#### 开闭原则

The Open/Closed Principle is about class design and feature extensions. It states that “Software entities (classes,
modules, functions, etc.) should be open for extension, but closed for modification.” This means that we should design
our modules, classes and functions in a way that when a new functionality is needed, we should not modify our existing
code but rather write new code that will be used by existing code. Practically speaking, this means that we should write
classes that implement and adhere to _interfaces_, then type-hint against those interfaces instead of specific classes.

The largest benefit of this approach is that we can very easily extend our code with support for something new without
having to modify existing code, meaning that we can reduce QA time, and the risk for negative impact to the application
is substantially reduced. We can deploy new code, faster, and with more confidence.

#### 里氏替换原则

The Liskov Substitution Principle is about subtyping and inheritance. It states that “Child classes should never break
the parent class’ type definitions.” Or, in Robert C. Martin’s words, “Subtypes must be substitutable for their base
types.”

For example, if we have a `FileInterface` interface which defines an `embed()` method, and we have `Audio` and `Video`
classes which both implement the `embed()` method, then we can expect that the usage of the `embed()` method will always
do the thing that we intend. If we later create a `PDF` class or a `Gist` class which implement the `FileInterface`
interface, we will already know and understand what the `embed()` method will do. The largest benefit of this approach
is that we have the ability to build flexible and easily-configurable programs, because when we change one object of a
type (e.g., `FileInterface`) to another we don't need to change anything else in our program.

#### 接口隔离原则

The Interface Segregation Principle (ISP) is about _business-logic-to-clients_ communication. It states that “No client
should be forced to depend on methods it does not use.” This means that instead of having a single monolithic interface
that all conforming classes need to implement, we should instead provide a set of smaller, concept-specific interfaces
that a conforming class implements one or more of.

For example, a `Car` or `Bus` class would be interested in a `steeringWheel()` method, but a `Motorcycle` or `Tricycle`
class would not. Conversely, a `Motorcycle` or `Tricycle` class would be interested in a `handlebars()` method, but a
`Car` or `Bus` class would not. There is no need to have all of these types of vehicles implement support for both
`steeringWheel()` as well as `handlebars()`, so we should break-apart the source interface.

#### 依赖倒置原则

The Dependency Inversion Principle is about removing hard-links between discrete classes so that new functionality can
be leveraged by passing a different class. It states that one should *"Depend on Abstractions. Do not depend on
concretions."*. Put simply, this means our dependencies should be interfaces/contracts or abstract classes rather than
concrete implementations. We can easily refactor the above example to follow this principle.

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(AdapterInterface $adapter)
    {
        $this->adapter = $adapter;
    }
}

interface AdapterInterface {}

class MysqlAdapter implements AdapterInterface {}
{% endhighlight %}

现在 `Database` 类依赖于接口，相比依赖于具体实现有更多的优势。

假设你工作的团队中，一位同事负责设计适配器。在第一个例子中，我们需要等待适配器设计完之后才能单元测试。现在由于依赖是一个接口/约定，我们能轻松地模拟接口测试，因为我们知道同事会基于约定实现那个适配器

这种方法的一个更大的好处是代码扩展性变得更高。如果一年之后我们决定要迁移到一种不同的数据库，我们只需要写一个实现相应接口的适配器并且注入进去，由于适配器遵循接口的约定，我们不需要额外的重构。
