d# <a name="main"></a> C++核心指南

March 7, 2019


作者:

* [Bjarne Stroustrup](http://www.stroustrup.com)
* [Herb Sutter](http://herbsutter.com/)

翻译：
* RealXie / 解玉坤

这是一个持续更新的文档，已经更新到0.8的版本，复制，使用，修改以及在此工程上的衍生创建都基于MIT风格的许可（license）。向本工程贡献内容需要同意贡献者许可（Contributor License），更多细节参考相关的[LICENSE](LICENSE) 文档。该项目允许友好的用户使用，复制，修改以及再创作，也期待有建设性的意见。

欢迎评论和建议，我们计划修改和扩展来提升对该语言的认识，以及改进可使用的library集合。当评论时请注明[the introduction](#S-introduction) 来概述我们的目的和通用方法。贡献者列表[here](#SS-ack)。

问题：
* 完成度，一致性和强制性的规则集合还未完全进行检查。
* ???来标记遗失的信息。
* 引用部分的更新；太多pre-c++资源过于陈旧。
* 或多或少的待定列表，见：[待定: 未分类的原型规则（protos-rule）](#S-unclassified)

您可以[read an explanation of the scope and structure of this Guide](#S-abstract)或直接跳到：

* [In: 介绍](#S-introduction)
* [P: 哲学](#S-philosophy)
* [I: ru](接口#S-interfaces)
* [F: 函数](#S-functions)
* [C: 类和类层次](#S-class)
* [Enum: 枚举](#S-enum)
* [R: 资源管理](#S-resource)
* [ES: 表达式和声明](#S-expr)
* [Per: 性能](#S-performance)
* [CP: 并发和并行](#S-concurrency)
* [E: 错误处理](#S-errors)
* [Con: 常量和不变性](#S-const)
* [T: 模板和泛型编程](#S-templates)
* [CPL: C风格编程](#S-cpl)
* [SF: 源文件](#S-source)
* [SL: 标准库](#S-stdlib)

Supporting sections:

* [A: 架构思想](#S-A)
* [NR: Non-Rules and myths](#S-not)
* [RF: 引用](#S-references)
* [Pro: Profiles](#S-profile)
* [GSL: 指南库](#S-gsl)
* [NL: 命名和布局规则](#S-naming)
* [FAQ: 常见问题解答](#S-faq)
* [附录 A: 库](#S-libraries)
* [附录 B: 现代化的代码](#S-modernizing)
* [附录 C: 讨论](#S-discussion)
* [附录 D: 支持工具](#S-tools)
* [术语](#S-glossary)
* [待定: 未分类的原型规则](#S-unclassified)


您可以摘取特定的语言我：

* 赋值:
[整齐类型](#Rc-regular) --
[prefer initialization](#Rc-initialize) --
[复制](#Rc-copy-semantic) --
[移动](#Rc-move-semantic) --
[其它操作](#Rc-matched) --
[默认值](#Rc-eqdefault)
* `类`:
[数据](#Rc-org) --
[不变量](#Rc-struct) --
[成员](#Rc-member) --
[helpers](#Rc-helper) --
[具体类型](#SS-concrete) --
[构造函数, =, 析构函数](#S-ctor) --
[层次](#SS-hier) --
[操作符](#SS-overload)
* `concept`:
[规则](#SS-concepts) --
[泛型编程](#Rt-raise) --
[模板参数](#Rt-concepts) --
[语义](#Rt-low)
* 构造函数:
[不变量](#Rc-struct) --
[创建不变量](#Rc-ctor) --
[`throw`](#Rc-throw) --
[默认值](#Rc-default0) --
[not needed](#Rc-default) --
[显式](#Rc-explicit) --
[代理](#Rc-delegating) --
[虚拟](#Rc-ctor-virtual)
* 派生类:
[何时需要](#Rh-domain) --
[作为接口](#Rh-abstract) --
[析构函数](#Rh-dtor) --
[复制](#Rh-copy) --
[getters and setters](#Rh-get) --
[多重继承](#Rh-mi-interface) --
[重载](#Rh-using) --
[切片](#Rc-copy-virtual) --
[`dynamic_cast`](#Rh-dynamic_cast)
* 析构函数:
[构造函数](#Rc-matched) --
[何时需要?](#Rc-dtor) --
[不应失败](#Rc-dtor-fail)
* 异常:
[errors](#S-errors) --
[`throw`](#Re-throw) --
[for errors only](#Re-errors) --
[`noexcept`](#Re-noexcept) --
[minimize `try`](#Re-catch) --
[what if no exceptions?](#Re-no-throw-codes)
* `for`:
[range-for and for](#Res-for-range) --
[for and while](#Res-for-while) --
[for-initializer](#Res-for-init) --
[empty body](#Res-empty) --
[loop variable](#Res-loop-counter) --
[loop variable type ???](#Res-???)
* function:
[naming](#Rf-package) --
[single operation](#Rf-logical) --
[no throw](#Rf-noexcept) --
[arguments](#Rf-smart) --
[argument passing](#Rf-conventional) --
[multiple return values](#Rf-out-multi) --
[pointers](#Rf-return-ptr) --
[lambdas](#Rf-capture-vs-overload)
* `inline`:
[small functions](#Rf-inline) --
[in headers](#Rs-inline)
* initialization:
[always](#Res-always) --
[prefer `{}`](#Res-list) --
[lambdas](#Res-lambda-init) --
[in-class initializers](#Rc-in-class-initializer) --
[class members](#Rc-initialize) --
[factory functions](#Rc-factory)
* lambda expression:
[when to use](#SS-lambdas)
* operator:
[conventional](#Ro-conventional) --
[avoid conversion operators](#Ro-conversion) --
[and lambdas](#Ro-lambda)
* `public`, `private`, and `protected`:
[information hiding](#Rc-private) --
[consistency](#Rh-public) --
[`protected`](#Rh-protected)
* `static_assert`:
[compile-time checking](#Rp-compile-time) --
[and concepts](#Rt-check-class)
* `struct`:
[for organizing data](#Rc-org) --
[use if no invariant](#Rc-struct) --
[no private members](#Rc-class)
* `template`:
[abstraction](#Rt-raise) --
[containers](#Rt-cont) --
[concepts](#Rt-concepts)
* `unsigned`:
[and signed](#Res-mix) --
[bit manipulation](#Res-unsigned)
* `virtual`:
[interfaces](#Ri-abstract) --
[not `virtual`](#Rc-concrete) --
[destructor](#Rc-dtor-virtual) --
[never fail](#Rc-dtor-fail)

You can look at design concepts used to express the rules:

* assertion: ???
* error: ???
* exception: exception guarantee (???)
* failure: ???
* invariant: ???
* leak: ???
* library: ???
* precondition: ???
* postcondition: ???
* resource: ???

# <a name="S-abstract"></a>摘要

本文档也是C++的指南集，该文档的目的是帮助大家更高效地使用现在C++，“现代”意味着有效的使用符合IOS的C++标准（目前是C++17，但大部分的建议也适用于C++14和C++11），换句话说，您希望5年后您的代码是什么样的？现在就开始吧。那么10年后呢？

该指南聚焦于相对高层次的问题，例如接口，资源管理，内存管理以及并发等，这些规则影响于应用架构和库的设计，相对于现今常用的规范，使用这些规则可以让代码静态类型安全，无资源泄漏，捕获更多的编程逻辑错误，并且可以让代码运行的更快，总之，值得您去做这些事情。

我们不太关注底层问题，如命名规范和对齐风格。然而，无用的废话是越线的。

我们最初的规则集合强化的安全性（多种形式）和简洁性，它们可以会太严格，希望没有引入更多的异常来适应现实的需求，但我们需要更多的规则。

您将会发现一些规则会违背您的预期，甚至违背您过往经历，如果我们不能用任何方式说服您改变您的编程风格，那么我们就失败了！请尝试来验证或否定这些规则。我们尤其希望能有一些测试或更好的案例来支持我们的规则。

您也发现一些明显的甚至琐碎的规则，请记住我们指南的目的是为了帮助那些缺乏经验的，其它语言转移过来的，或者需要来提升该语言的人。

大多的规则被设计成可以被分析工具来支持，违反的规则被标记出（链接）到相应的规则。我们不希望您在写代码之前记住所有的规则，一种理解是可以把该文档当成碰巧可以被阅读的（分析）工具的说明书。

这些规则用于逐步引入代码库，我们计划为此开发工具，并希望其他人也能这样做。

欢迎对改进提出意见和建议，随着我们对该文档的理解提高，以及语言和可用库集的改进，我们计划修改和扩展该文档。

# <a name="S-introduction"></a>In: 介绍

这是一组针对现代c++(目前c++17)的核心指南，考虑了将来可能的增强和ISO技术规范(TSs)。
其目的是帮助c++程序员编写更简单、更高效、更易于维护的代码。

简介:

* [目标读者](#SS-readers)
* [目标](#SS-aims)
* [In.not: Non-aims](#SS-non)
* [In.强制: 强制](#SS-force)
* [In.结构: 文档结构](#SS-struct)
* [In.sec: 主要部分](#SS-sec)

## <a name="SS-readers"></a>目标读者

所有的C++编程人员，包括[可能会考虑C的程序员](#S-cpl)。

## <a name="SS-aims"></a>目标

本文档的目的是为了帮助开发者使用现代C++（目前C++17）和在不同代码库之间达成更为统一的风格。

不要产生每条规则都可以有效地适用每个代码库的错觉，旧有的代码库很难升级，但我们相信使用规则的程序更不易于出错，更具可维护性，通常规则也使得初始开发更加快速和容易。至少我们可以说，这些规则可以让代码性能至少像古老的甚至更传统的技术一样好，或者更优秀。使用这些零开销的原则（”不使用，不花钱“，或者当你适当地使用抽象机制时，至少可以获得与使用低级语言结构手工编码时一样好的性能）。这些规则是新代码的理想规则，在处理旧代码是这发挥的机会，尽可以地接近这些理想的规则。

### <a name="R0"></a>别慌

花点时间来理解指引规则对您程序的意义。

这些指南遵循“超集的子集”这一原则[Stroustrup05](#Stroustrup05)，它们并非简单的定义一个C++的子集来使用（为了可靠性，安全性，性能或者其它什么），相反，它们强烈推荐使用一些简单的“扩展”([库组件](#S-gsl))，这使得使用c++中最容易出错的特性变得多余，因此它们可以被禁止(在我们的规则集中)。

规则强调静态类型安全性和资源安全性。
因此，他们强调范围检查的可能性，避免取消对“nullptr”的引用，避免悬空指针，以及系统地使用异常(通过RAII)。
部分是为了实现这一点，部分是为了将晦涩的代码作为错误的来源最小化，规则还强调了简单性和在良好指定的接口后面隐藏必要的复杂性。

这些规则强化了静态类型安全和资源安全，因此它们强调了范围检查的可能性，避免对‘nullptr’进行解引用，避免悬垂指针，以及系统地使用异常（通过RAII）。部分是为了实现这一点，部分是为了将晦涩的代码作为错误的来源最小化，规则还强调了简单性和在良好指定的接口后面隐藏必要的复杂性。

许多规则都是规范性的。
我们对那些只说“不要那样做!”而不提供其他选择的规则感到不舒服。
这样做的一个结果是，某些规则只能由启发式来支持，而不能由精确且可自动验证的检查来支持。
其他规则阐明了一般原则，对于这些更通用，更详细和具体的规则则提供部分检查。

这些指南指出了C++的核心及作用，我们期望大型组织，特定的应用领域，甚至大的项目需要更多的规则，或许是更多的限制和更多的库支持。例如，硬（件）实时程序通常不能使用自由地自由存储（动态内存），并且限度库的使用选择。我们鼓励制定更为特定的规则，作为这些核心指南的补充。构建您理想的小型基础库并使用它，而不是使用更低层次的美化汇编代码。

这些规则设计初衷允许[逐步适配](#S-modernizing).

一些规则旨在增加各种形式的安全性，而另一些旨在减少事故的可能性，许多两者兼而有之。
旨在防止事故发生的指导方针常常禁止完全合法的c++。
然而，当有两种表达思想的方法时，一种方法显示出常见的错误来源，而另一种方法没有，我们试图引导程序员使用后者。

## <a name="SS-non"></a>不是我们的目标

并不打算让这些规则是最小或正交的，特别是，通用规则可能很简单，但无法强制执行，而且，通常很难理解一般规则的含义。更细化的规则通常更容易理解和执行，但是如果没有通用规则，它们只是一长串特殊情况的列表。我们旨在提供能帮助新手和支持专家使用的规则，有些规则可以完全强制执行，但另一些则是启发式的。

These rules are not meant to be read serially, like a book.
You can browse through them using the links.
However, their main intended use is to be targets for tools.
That is, a tool looks for violations and the tool returns links to violated rules.
The rules then provide reasons, examples of potential consequences of the violation, and suggested remedies.

These guidelines are not intended to be a substitute for a tutorial treatment of C++.
If you need a tutorial for some given level of experience, see [the references](#S-references).

This is not a guide on how to convert old C++ code to more modern code.
It is meant to articulate ideas for new code in a concrete fashion.
However, see [the modernization section](#S-modernizing) for some possible approaches to modernizing/rejuvenating/upgrading.
Importantly, the rules support gradual adoption: It is typically infeasible to completely convert a large code base all at once.

These guidelines are not meant to be complete or exact in every language-technical detail.
For the final word on language definition issues, including every exception to general rules and every feature, see the ISO C++ standard.

The rules are not intended to force you to write in an impoverished subset of C++.
They are *emphatically* not meant to define a, say, Java-like subset of C++.
They are not meant to define a single "one true C++" language.
We value expressiveness and uncompromised performance.

The rules are not value-neutral.
They are meant to make code simpler and more correct/safer than most existing C++ code, without loss of performance.
They are meant to inhibit perfectly valid C++ code that correlates with errors, spurious complexity, and poor performance.

The rules are not precise to the point where a person (or machine) can follow them blindly.
The enforcement parts try to be that, but we would rather leave a rule or a definition a bit vague
and open to interpretation than specify something precisely and wrong.
Sometimes, precision comes only with time and experience.
Design is not (yet) a form of Math.

The rules are not perfect.
A rule can do harm by prohibiting something that is useful in a given situation.
A rule can do harm by failing to prohibit something that enables a serious error in a given situation.
A rule can do a lot of harm by being vague, ambiguous, unenforceable, or by enabling every solution to a problem.
It is impossible to completely meet the "do no harm" criteria.
Instead, our aim is the less ambitious: "Do the most good for most programmers";
if you cannot live with a rule, object to it, ignore it, but don't water it down until it becomes meaningless.
Also, suggest an improvement.

## <a name="SS-force"></a>In.force: Enforcement

Rules with no enforcement are unmanageable for large code bases.
Enforcement of all rules is possible only for a small weak set of rules or for a specific user community.

* But we want lots of rules, and we want rules that everybody can use.
* But different people have different needs.
* But people don't like to read lots of rules.
* But people can't remember many rules.

So, we need subsetting to meet a variety of needs.

* But arbitrary subsetting leads to chaos.

We want guidelines that help a lot of people, make code more uniform, and strongly encourage people to modernize their code.
We want to encourage best practices, rather than leave all to individual choices and management pressures.
The ideal is to use all rules; that gives the greatest benefits.

This adds up to quite a few dilemmas.
We try to resolve those using tools.
Each rule has an **Enforcement** section listing ideas for enforcement.
Enforcement might be done by code review, by static analysis, by compiler, or by run-time checks.
Wherever possible, we prefer "mechanical" checking (humans are slow, inaccurate, and bore easily) and static checking.
Run-time checks are suggested only rarely where no alternative exists; we do not want to introduce "distributed fat".
Where appropriate, we label a rule (in the **Enforcement** sections) with the name of groups of related rules (called "profiles").
A rule can be part of several profiles, or none.
For a start, we have a few profiles corresponding to common needs (desires, ideals):

* **type**: No type violations (reinterpreting a `T` as a `U` through casts, unions, or varargs)
* **bounds**: No bounds violations (accessing beyond the range of an array)
* **lifetime**: No leaks (failing to `delete` or multiple `delete`) and no access to invalid objects (dereferencing `nullptr`, using a dangling reference).

The profiles are intended to be used by tools, but also serve as an aid to the human reader.
We do not limit our comment in the **Enforcement** sections to things we know how to enforce; some comments are mere wishes that might inspire some tool builder.

Tools that implement these rules shall respect the following syntax to explicitly suppress a rule:

    [[gsl::suppress(tag)]]

where "tag" is the anchor name of the item where the Enforcement rule appears (e.g., for [C.134](#Rh-public) it is "Rh-public"), the
name of a profile group-of-rules ("type", "bounds", or "lifetime"),
or a specific rule in a profile ([type.4](#Pro-type-cstylecast), or [bounds.2](#Pro-bounds-arrayindex)).

## <a name="SS-struct"></a>In.struct: The structure of this document

Each rule (guideline, suggestion) can have several parts:

* The rule itself -- e.g., **no naked `new`**
* A rule reference number -- e.g., **C.7** (the 7th rule related to classes).
  Since the major sections are not inherently ordered, we use letters as the first part of a rule reference "number".
  We leave gaps in the numbering to minimize "disruption" when we add or remove rules.
* **Reason**s (rationales) -- because programmers find it hard to follow rules they don't understand
* **Example**s -- because rules are hard to understand in the abstract; can be positive or negative
* **Alternative**s -- for "don't do this" rules
* **Exception**s -- we prefer simple general rules. However, many rules apply widely, but not universally, so exceptions must be listed
* **Enforcement** -- ideas about how the rule might be checked "mechanically"
* **See also**s -- references to related rules and/or further discussion (in this document or elsewhere)
* **Note**s (comments) -- something that needs saying that doesn't fit the other classifications
* **Discussion** -- references to more extensive rationale and/or examples placed outside the main lists of rules

Some rules are hard to check mechanically, but they all meet the minimal criteria that an expert programmer can spot many violations without too much trouble.
We hope that "mechanical" tools will improve with time to approximate what such an expert programmer notices.
Also, we assume that the rules will be refined over time to make them more precise and checkable.

A rule is aimed at being simple, rather than carefully phrased to mention every alternative and special case.
Such information is found in the **Alternative** paragraphs and the [Discussion](#S-discussion) sections.
If you don't understand a rule or disagree with it, please visit its **Discussion**.
If you feel that a discussion is missing or incomplete, enter an [Issue](https://github.com/isocpp/CppCoreGuidelines/issues)
explaining your concerns and possibly a corresponding PR.

This is not a language manual.
It is meant to be helpful, rather than complete, fully accurate on technical details, or a guide to existing code.
Recommended information sources can be found in [the references](#S-references).

## <a name="SS-sec"></a>In.sec: 主要部分

* [In: 介绍](#S-introduction)
* [P: 哲学](#S-philosophy)
* [I: 接口](#S-interfaces)
* [F: 函数](#S-functions)
* [C: 类和类层次结构](#S-class)
* [Enum: 枚举](#S-enum)
* [R: 资源管理](#S-resource)
* [ES: 表达式和声明](#S-expr)
* [Per: 性能](#S-performance)
* [CP: 并发和并行](#S-concurrency)
* [E: 错误处理](#S-errors)
* [Con: 常量和不变性](#S-const)
* [T: 模板和泛型编程](#S-templates)
* [CPL: C风格编程](#S-cpl)
* [SF: Source files](#S-source)
* [SL: 标准库](#S-stdlib)

辅助部分:

* [A: 架构思想](#S-A)
* [NR: Non-Rules and myths](#S-not)
* [RF: 引用](#S-references)
* [Pro: Profiles](#S-profile)
* [GSL: 指南库](#S-gsl)
* [NL: 命名和布局规则](#S-naming)
* [FAQ: 常见问题解答](#S-faq)
* [附录 A: 库](#S-libraries)
* [附录 B: 现代化的代码](#S-modernizing)
* [附录 C: 讨论](#S-discussion)
* [附录 D: 支持工具](#S-tools)
* [术语](#S-glossary)
* [To-do: Unclassified proto-rules](#S-unclassified)

这些章节之间不是正交的（译注：完全独立的）。

每个章节(如"P"表示“哲学”)和子章节（如"c.hier"表示"类层次结构(OOP)"）都有一个便于搜索和引用的缩写，主要章节的缩写也会使用规则编号(如"C.11"表示“规范具体类型”)。

# <a name="S-philosophy"></a>P: 哲学

这部分规则是非常通用的，这些哲学规则的概述：

* [P.1: 代码中直接表明意图](#Rp-direct)
* [P.2: 使用符合IOS标准的C++](#Rp-Cplusplus)
* [P.3: 表达意图](#Rp-what)
* [P.4: 理想情况下，程序应当是静态类型安全的](#Rp-typesafe)
* [P.5: 优先编译时检查而不是运行时检查](#Rp-compile-time)
* [P.6: 编译时无法做的检查应当在运行时做](#Rp-run-time)
* [P.7: 提前捕获运行时错误](#Rp-early)
* [P.8: 勿泄漏任务资源](#Rp-leak)
* [P.9: 不要浪费时间和空间](#Rp-waste)
* [P.10: 优先使用不可变数据，而非可变数据](#Rp-mutable)
* [P.11: 封装混乱的结果，而不是分散在代码中](#Rp-library)
* [P.12: 适当使用支持工具](#Rp-tools)
* [P.13: 适当使用支持库](#Rp-lib)

哲学性的规则一般不能机械化地检查，然而，独立的规则反映了这些哲学主题。
没有哲学基础，更具体/特定/可检查的规则会缺乏基本原理的支持。

### <a name="Rp-direct"></a>P.1: 代码中直接表明意图

##### 原因

编译器不会读取注释（或者设计文档），相应地很多程序员也不会读。代码中表达的确定性语义原则上可以使用编译器或者其它工具进行检查。

##### 例子

    class Date {
        // ...
    public:
        Month month() const;  // do
        int month();          // don't
        // ...
    };

第一个‘month’的声明明确返回一个‘month’，并且不会修改‘Date’对象的状态，而第二个则让读者去猜测，并为未捕获的bug提供了更多的可能性。

##### 糟糕的例子

该循环是`std::find`的受限形式：


    void f(vector<string>& v)
    {
        string val;
        cin >> val;
        // ...
        int index = -1;                    // bad, plus should use gsl::index
        for (int i = 0; i < v.size(); ++i) {
            if (v[i] == val) {
                index = i;
                break;
            }
        }
        // ...
    }

##### 好的例子

更清晰地表达意图：

    void f(vector<string>& v)
    {
        string val;
        cin >> val;
        // ...
        auto p = find(begin(v), end(v), val);  // better
        // ...
    }

一个设计良好的库表达的意图（将要做什么，而不仅仅是做了什么）远好于直接使用语言特性。

c++程序员应该了解标准库的基础知识，并在适当的地方使用它，任何程序员都应该了解并适当使用正在处理的项目的基础库的基础，任何使用这些指南的程序员都应该知道[指南支持库](#S-gsl)，并正确地使用它。

##### 例子

    change_speed(double s);   // 糟糕: s指什么?
    // ...
    change_speed(2.3);

更好的方法是明确double和单位的意义（新速度，或旧速度`差值`？）

    change_speed(Speed s);    // 好的: 指出的s的意图
    // ...
    change_speed(2.3);        // 错误: 没有单位
    change_speed(23m / 10s);  // 米/秒

我们可以接受仅仅（无单位）使用`double`作为差值，但这将会易于出错。如果我们想要绝对速度和差值，可以定义一个`Delta`类型。

##### 实施

通常很难

* 始终使用`const`(检查成员函数是否修改它们的对象；检查函数是否修改了以指针或引用传递的参数)
* 使用强制类型转换标记（casts neuter the type system）
* 检测模仿了标准库的代码（很难）

### <a name="Rp-Cplusplus"></a>P.2: 使用符合IOS标准的C++

##### 原因

这是一个写符合IOS标准的C++的指南。

##### 注意

有些情况下扩展是必须的，例如访问系统资源，在这种情况下，本地化必要扩展的使用，并使用非核心编码准则控制它们。如果可能，构建扩展的接口的封装，以便在不支持这些扩展的系统上关闭或编译它们。

Extensions often do not have rigorously defined semantics.  Even extensions that
are common and implemented by multiple compilers may have slightly different
behaviors and edge case behavior as a direct result of *not* having a rigorous
standard definition.  With sufficient use of any such extension, expected
portability will be impacted.


##### 注意

使用合法的IOS C++并不能保证可移植性（更不用说正确性），避免依赖不确定的行为(例如, [未定义的求值顺序](#Res-order))，注意由实现（`implementation`）来定义意义的构造（例如，`sizeof(int)`）。

##### 注意

在某些环境中，限制条件在使用标准c++或库特性时是必要的，例如，在飞机控制软件标准要求避免使用动态内存分配。在这种特殊情况下，控制它们使用（或不使用）这些编码指南的扩展。

##### 强制执行

使用一组最新的不接受扩展选项的C++编译器（目前C++17, C++14，或C++11）。

### <a name="Rp-what"></a>P.3: 表达意图

##### 原因

除非一代码的意图被注明（例如，名字或注释），否则不可能判断代码是否做了它被猜测做的事。

##### Example

    gsl::index i = 0;
    while (i < v.size()) {
        // ... do something with v[i] ...
    }

这里并没有表达仅仅遍历`v`的意图，`index`的实现细节被公开（所以可能被误用）。`i`的生命周期有意无意的超过了循环本身，读者无法仅仅从这段代码做出判断。

更好的做法:

    for (const auto& x : v) { /* do something with the value of x */ }

现在没有显示地提到迭代机制，在`const`引用上的循环操作可以避免无意的改动，如果希望修改，这样写：

    for (auto& x : v) { /* modify x */ }

更多for语句的细节见[ES.71](#Res-for-range)，有时使用具名算法会更好，这里使用了`Ranges TS`中的`for_each`，直接表达了意图：

    for_each(v, [](int x) { /* do something with the value of x */ });
    for_each(par, v, [](int x) { /* do something with the value of x */ });

最后一种变体明确表明我们对要处理的`v`的元素的顺序不感兴趣。

程序需要熟悉这些

* [指南支持库(gsl)](#S-gsl)
* [C++标准库](#S-stdlib)
* 当前项目使用的任意基础库

##### 注意

替代方案:说应该做什么，而不仅仅是应该怎么做。

##### 注意

一些语言结构比另一些更好地表达意图。

##### 例子

如果两个`int'指的是一个2D点的坐标，就这样写：

    draw_line(int, int, int, int);  // 晦涩
    draw_line(Point, Point);        // 清晰


##### 实施

寻找有更好替代方案的常见模式

* 简单的`for`循环 vs. 基于Range的`for`循环
* `f(T*, int)` 接口 vs. `f(span<T>)` 接口
* 太大范围的循环变量
* 裸露的 `new` 和 `delete`
* 太多内置类型参数的函数

这给了智能和半自动程序转换的巨大空间。

### <a name="Rp-typesafe"></a>P.4: 理想情况下，程序应当是静态类型安全的

##### 原因

理想情况下，程序应该完全静态（编译时）类型安全的，然而不幸的是，这是不可能的，有这些问题

* 联合体
* 强制类型转换
* 数组衰退（decay）
* 范围错误
* 收缩转换（float -> int）

##### 注意

这些都是严重问题的根源（例如，crash和安全性问题），我们会尝试提供替代技术。

##### 实施


我们可以根据个别程序的需求和可行性分别禁止、限制或检测个别问题类别。总会建议一种替代方案，例如：

* 联合体 -- 使用 `variant` (C++17)
* 强制类型转换 -- 最小化使用它们; 使用template来获取帮助
* 数组衰退 -- 使用 `span` (来自GSL)
* 范围错误 -- 使用 `span`
* 收缩转换 -- 最小化使用它们；当需要时，使用`narrow`或者`narrow_cast` (GSL)

### <a name="Rp-compile-time"></a>P.5: 优先编译时检查而不是运行时检查

##### 原因

代码清晰而高效，无需要为编译时错误而编写错误处理程序。

##### Example

    // Int 是整型的别名
    int bits = 0;         // don't: avoidable code
    for (Int i = 1; i; i <<= 1)
        ++bits;
    if (bits < 32)
        cerr << "Int too small\n";

这个例子无法获取想要的效果（因为溢出是未定义的），应当被替换成使用简单的`static_assert`:

    // Int is an alias used for integers
    static_assert(sizeof(Int) >= 4);    // do: compile-time check

或者更好地使用类型系统，以及使用`int32_t`来替换`int`。

##### 例子

    void read(int* p, int n);   // 读取n个整数到*p

    int a[100];
    read(a, 1000);    // bad, off the end

更好的做法：

    void read(span<int> r); // read into the range of integers r

    int a[100];
    read(a);        // 好处: 让编译器计算出元素的个数

**替代方案**: 不要将可以在编译时做好的事情推迟到运行时做。

##### 执行

* 寻找指针类型的参数。
* 寻找运行时的范围越界检查。

### <a name="Rp-run-time"></a>P.6: 编译时无法做的检查应当在运行时做

##### 原因

留下难以检测的错误在程序中会导致崩溃和糟糕的结果。

##### Note

理想情况下我们在编译时或运行时捕获所有的错误（非程序员的逻辑错误），然而并不可能在编译时捕获所有的错误，通常也不值得在运行捕获所有剩余的错误，然而我应当尽力编写在给定充足资源（分析程序，运行时检查，机器资源，时间等）的情况下原则上可以被检查的代码。

##### 糟糕的例子

    // 分开编译，可能会动态加载
    extern void f(int* p);

    void g(int n)
    {
        // 糟糕: 元素的数量没有传递给 f()
        f(new int[n]);
    }

在这里，元素的数量这一个关键信息被彻底“模糊”了，以至于静态分析可能变得不再可行，当“f()”是ABI的一部分时可能会让动态检查非常困难，以至于不能“检测”这个指针。我在免费存储中嵌入有用的信息，但这需要对系统进行全局更改，也许还需要对编译器进行更改，这样的设计使得错误检测非常困难。

##### 糟糕的例子

当然，我们也可以将无数的数据随指针一起传进去：

    // separately compiled, possibly dynamically loaded
    extern void f2(int* p, int n);

    void g2(int n)
    {
        f2(new int[n], m);  // 糟糕：可能会将错误的无数数量传给f()
    }

将元素的数量作为参数传递要比仅仅传递指针并依赖某种(未声明的)约定来了解或发现元素的数量更好(而且更常见)。然而(如所示)，一个简单的拼写错误可能会导致严重的错误。`f2()`的两个参数之间的联系是习惯性的，而不是显式的。

同样，它也暗示着`f2()`支持`delete`它的参数（或者，这是调用者犯的第二个错误？）。


##### 糟糕的例子

标准库的资源管理指针指向对象时也无法传递大小：

    // 分开编译，可能动态加载
    // NB: 假设这里的调用代码是 ABI-兼容的， 使用兼容的C++编译器以及相同的stdlib实现
    extern void f3(unique_ptr<int[]>, int n);

    void g3(int n)
    {
        f3(make_unique<int[]>(n), m);    // 糟糕: 分开传递所有权和大小
    }

##### Example

我们需要将指针和元素个数作为一个整体对象进行传递:

    extern void f4(vector<int>&);   // 分开编译，可能动态加载
    extern void f4(span<int>);      // 分开编译，可能动态加载
                                    // NB: 假设这里的调用代码是 ABI-兼容的， 使用兼容的C++编译器以及相同的stdlib实现

    void g3(int n)
    {
        vector<int> v(n);
        f4(v);                     // 传递一个引用，保留所有权(ownership)
        f4(span<int>{v});          // 传递一个视图(view)，保留所有权
    }

这种设计将元素的数量作为对象的一个组成部分，这样错误就不太可能发生，如果条件允许，动态(运行时)检查也是可行的。

##### Example

我们如何同时传递所有权和验证所需的所有信息?

    vector<int> f5(int n)    // OK: move
    {
        vector<int> v(n);
        // ... initialize v ...
        return v;
    }

    unique_ptr<int[]> f6(int n)    // 糟糕: 丢失了`n`
    {
        auto p = make_unique<int[]>(n);
        // ... initialize *p ...
        return p;
    }

    owner<int*> f7(int n)    // 糟糕: 丢失了`n`，并且可能会忘记`delete`
    {
        owner<int*> p = new int[n];
        // ... initialize *p ...
        return p;
    }

##### Example

* ???
* show how possible checks are avoided by interfaces that pass polymorphic base classes around, when they actually know what they need?
  Or strings as "free-style" options

##### Enforcement

* (指针，个数)类型的操口（这可以给出非常多的由于兼容性而无法修复的例子）
* ???

### <a name="Rp-early"></a>P.7: 提前捕获运行时错误

##### Reason

避免“神秘的”崩溃，避免由错误导致的（不可感知的）错误结果。

##### Example

    void increment1(int* p, int n)    // 糟糕: 易于出错
    {
        for (int i = 0; i < n; ++i) ++p[i];
    }

    void use1(int m)
    {
        const int n = 10;
        int a[n] = {};
        // ...
        increment1(a, m);   // 可能是拼写错误，或许m<=n是被支持的，但是假设 m==20
        // ...
    }

Here we made a small error in `use1` that will lead to corrupted data or a crash.
The (pointer, count)-style interface leaves `increment1()` with no realistic way of defending itself against out-of-range errors.
If we could check subscripts for out of range access, then the error would not be discovered until `p[10]` was accessed.
We could check earlier and improve the code:

我们在`use1`中一个引起数据损坏或者崩溃的小错误，(指针，个数)样式的接口使得`increment1()`没有切实可行的方法来防止数组访问越界的问题。如果我们使用下标检查来防止越界访问，那么这个错误只有在`p[10]`被访问时才会被发现。我可以提前检查来改进代码：

    void increment2(span<int> p)
    {
        for (int& x : p) ++x;
    }

    void use2(int m)
    {
        const int n = 10;
        int a[n] = {};
        // ...
        increment2({a, m});    // 可能是拼写错误，可能m< = n是被支持的
        // ...
    }

现在，可以在调用点(早期)而不是稍后检查`m <= n`，如果仅有一个拼写错误，并打算使用`n`作为边界，那么代码可以更加简洁（消除错误的可能性）：

    void use3(int m)
    {
        const int n = 10;
        int a[n] = {};
        // ...
        increment2(a);   // 元素的个数不需要重复
        // ...
    }

##### 糟糕的例子

不要重复检查同一个值，不要以字符串的形式传递结构化的数据：

    Date read_date(istream& is);    // 从istream中读取`date`

    Date extract_date(const string& s);    // 从字符串的提取`date`

    void user1(const string& date)    // 操作`date`
    {
        auto d = extract_date(date);
        // ...
    }

    void user2()
    {
        Date d = read_date(cin);
        // ...
        user1(d.to_string());
        // ...
    }

date被做了两次的合法性验证（`Date`的构造函数），并以字符串(非结构化数据)的形式进行传递(参数)。

##### Example

过多的检查可能是昂贵的。在某些情况下，提前检查是愚蠢的，因为您可能永远不需要该值，或者只需要检查比整个值更容易检查的部分。类似地，不要添加更改接口渐近行为的有效性检查(例如，不要将`O(n)`的检查添加到平均复杂度为`O(1)`的接口中)。

    class Jet {    // Physics says: e * e < x * x + y * y + z * z
        float x;
        float y;
        float z;
        float e;
    public:
        Jet(float x, float y, float z, float e)
            :x(x), y(y), z(z), e(e)
        {
            // 我应该检查这些值是具有实际(物理)意义的吗？
        }

        float m() const
        {
            // 我应该处理这种退化(简化)的情形吗？
            return sqrt(x * x + y * y + z * z - e * e);
        }

        ???
    };

由于存在测量误差的可能性，(`e * e < x * x + y * y + z * z`)对于喷气式飞机的物理规律而言并不是不变的。

???

##### 实施

* 查看指针和数组：提前进行范围检查，并且不要重复检查
* 查看转换：:消除或标记收缩转换
* 查看输入的未检查值
* 查看结构性数据(含有不变量的类对象)被转换成字符串
* ？？？

### <a name="Rp-leak"></a>P.8: 勿泄漏任务资源

##### 原因

随着时间的推移，即使是资源的缓慢增长也会耗尽这些资源的可用性，这对于长时间运行的程序特别重要，但也是负责任的编程行为的基本部分。


##### 糟糕的例子

    void f(char* name)
    {
        FILE* input = fopen(name, "r");
        // ...
        if (something) return;   // 糟糕：如果 something == true，文件句柄就泄漏了
        // ...
        fclose(input);
    }

优先使用[RAII](#Rr-raii):

    void f(char* name)
    {
        ifstream input {name};
        // ...
        if (something) return;   // OK: 没有泄漏
        // ...
    }

**参考**: [资源管理部分](#S-resource)

##### 注意

泄漏通俗地说就是“任何没有被清理的东西”，更重要的分类是“任何不能再被清理的东西”。例如，在堆上分配了一个对象，然而丢失了指向该块内存(分配)的指针。
不应将此规则应用在程序关闭期间需返回长生命周期对象的分配（译注：在程序退出期间的内存分配可以不需要遵守这些规则），例如，依赖于系统保证的清理可以简化代码，比如关闭文件和在进程关闭时释放内存。然而，依赖于隐式清理的抽象也同样简单，而且通常更安全。

##### 注意

使用[生命周期安全性剖面(profile)](#SS-lifetime) 来消除泄漏，当与[RAII](#Rr-raii)提供的资源安全相结合时，就没了"垃圾回收"的需求。当使用[类型和边界剖面（profiles）](#SS-force)时，你可以获得由工具保证的类型和资源安全。

##### 实施

* 查看指针：将它们分为非所有者(默认)和所有者(owner)，在可行的地方，用标准库资源句柄替换所有者(如上面的示例所示)，或者使用[GSL](#S-gsl)中的`owner`来标记所有者。
* 查看裸用的`new`和`delete`
* 查看已知返回原始指针的资源分配函数(如`fopen`, `malloc` 和 `strdup`)

### <a name="Rp-waste"></a>P.9: 不要浪费时间和空间

##### 原因

这是 C++.

##### 注意

为了实现目标而花费的时间和空间(如，开发速度、资源安全性或测试的简化)不是浪费，”追求效率的另一个好处是，这个过程迫使你更深入地理解问题“ -- Alex Stepanov。

##### 糟糕的例子

    struct X {
        char ch;
        int i;
        string s;
        char ch2;

        X& operator=(const X& a);
        X(const X&);
    };

    X waste(const char* p)
    {
        if (!p) throw Nullptr_error{};
        int n = strlen(p);
        auto buf = new char[n];
        if (!buf) throw Allocation_error{};
        for (int i = 0; i < n; ++i) buf[i] = p[i];
        // ... 操作 buffer ...
        X x;
        x.ch = 'a';
        x.s = string(n);    // 为*p而分配的x.s空间
        for (gsl::index i = 0; i < x.s.size(); ++i) x.s[i] = buf[i];  // 将buf拷贝到x.s
        delete[] buf;
        return x;
    }

    void driver()
    {
        X x = waste("Typical argument");
        // ...
    }

是的，这只是一个夸张的描述，但是我们已经看到了生产代码中的每个错误，甚至更糟的问题。注意，`X`的布局至少浪费6个字节(很可能更多)，虚假的复制操作定义禁用了move语义，从而使得返回操作很慢(请注意，这里不保证返回值优化RVO)。为`buf`而使用`new`和`delete`也是多余的，如果我们真的需要一个局部字符串，我们应该使用一个局部`string`。还有更多的性能bug和不必要(gratuitous)的复杂性。

##### 糟糕的例子

    void lower(zstring s)
    {
        for (int i = 0; i < strlen(s); ++i) s[i] = tolower(s[i]);
    }

是的， 这是一个生产代码的示例，我们把它留给读者去找出什么被浪费了。

##### 注意

关于浪费，孤立的示例通常是不重要的，如果重要，通常也很容易被专家来消除。然而，在代码库中大量传播的浪费很容易造成严重后果，而且专家并不总是像我们希望的那样可用。这个规则(以及支持它的更具体的规则)的目的是在使用c++之前消除与之相关的大部分浪费。之后，我们可以查看与算法和需求相关的浪费，但这超出了本指南的范围。

##### 实施

许多更具体的规则旨在实现简化和消除不必要浪费的总体目标。

* 从用户定义的非默认后缀`operator++`或`operator--`函数中标记一个未使用的返回值，优先使用前缀形式(注:“用户自定义非默认值”是为了减少噪音。如果在实践中仍然过于嘈杂，请重新审视这一措施。)


### <a name="Rp-mutable"></a>P.10: 优先使用不可变数据，而非可变数据

##### 原因

对常量进行推理比变量容易，不可变的东西不会意外地改变，有时不变性可以实现更好的优化，常量上也不会出现数据竞争(data race)的问题。

参见 [Con: 常量和不变性](#S-const)

### <a name="Rp-library"></a>P.11: 封装混乱的结果，而不是分散在代码中

##### 原因

混乱的代码更难于编写且有可能隐藏bug，一个好的接口使用起来更容易且安全，混乱的底层代码会产生更多混乱的代码。

##### 示例

    int sz = 100;
    int* p = (int*) malloc(sizeof(int) * sz);
    int count = 0;
    // ...
    for (;;) {
        // ... 读取一个int到x中，如果到达文件结尾则退出循环...
        // ... 检查x是合法的 ...
        if (count == sz)
            p = (int*) realloc(p, sizeof(int) * sz * 2);
        p[count++] = x;
        // ...
    }

这是低级的、冗长的、容易出错的，例如，我们“忘记”测试内存不足。相反，我们可以用vector：

    vector<int> v;
    v.reserve(100);
    // ...
    for (int x; cin >> x; ) {
        // ... 检查x是合法的 ...
        v.push_back(x);
    }

##### 注意

标准库和GSL就是这种理念的例子，例如，我们使用`vector`、`span`、`lock_guard`和`future`来实现关键抽象，而不是处理数组、联合体、强制转换、棘手的生存期问题、“gsl::owner”等等。我们使用那些拥有更多时间和专业知识的人设计和实现的库，类似地，我们可以而且应该设计和实现更专门化的库，而不是让用户(通常是我们自己)不断地把底层代码弄好，这是[超集原则子集](#R0)的变体，它是这些指导原则的基础。


##### 实施

* 查看“混乱的代码”，比如复杂的指针操作和抽象实现之外的强制转换。


### <a name="Rp-tools"></a>P.12: 适当使用支持工具

##### 原因

有很多事情“机器”会做得更好，电脑不会因重复的任务而感到疲劳或厌烦，我们通常有比重复的日常任务更好的事情要做。

##### 示例

运行一个静态分析器来验证您的代码是否遵循您希望它遵循的准则。

##### Note

参见

* [Static analysis tools](???)
* [Concurrency tools](#Rconc-tools)
* [Testing tools](???)

还有许多其他类型的工具，比如代码仓库、构建工具等等，但是这些都超出了本指南的范围。

##### 注意

注意不要依赖于过于精细或专业化的工具链，否则，您的可移植代码将变得不可移植。


### <a name="Rp-lib"></a>P.13: 适当使用支持库

##### 原因

使用设计良好、文档丰富和支持良好的库可以节省时间和精力；如果您的大部分时间必须花在实现上，那么它的质量和文档可能比您所能做的更好。库的成本(时间、精力、金钱等)可以由多个用户平摊。与单个应用程序相比，广泛使用的库更有可能保持更新并移植到新系统。了解广泛使用的库可以在其他/将来的项目上节省时间。因此，如果存在适合您的应用程序域的库，那么就使用它。

##### 示例

    std::sort(begin(v), end(v), std::greater<>());

除非您是排序算法方面的专家，并且有足够的时间，否则这比为特定应用程序编写的任何东西都更有可能是正确的，并且运行得更快。您需要一个不使用标准库(或应用程序使用的任何基础库)的理由，而不是一个使用它的理由。

##### 注意

使用默认的库

* The [IOS C++的标准库](#S-stdlib)
* The [GSL:指南支持库](#S-gsl)

##### 注意

如果在重要领域没有设计良好，文档良好和支持良好的库，你应该去设计、实现并使用它。


# <a name="S-interfaces"></a>I: 接口

接口是程序两部分之间的契约，准确地说明对服务供应商和该服务用户的期望是至关重要的。良好的接口(易于理解、鼓励高效使用、不容易出错、支持测试等等)可能是代码组织中最重要的一个方面。

接口规则总结:

* [I.1: 使接口明确](#Ri-explicit)
* [I.2: 避免非`const`的全局变量](#Ri-global)
* [I.3: 避免单例](#Ri-singleton)
* [I.4: 使接口精确且强类型](#Ri-typed)
* [I.5: 声明先决条件 (if any)](#Ri-pre)
* [I.6: 优先使用 `Expects()` 来表达先决条件](#Ri-expects)
* [I.7: 声明后置条件](#Ri-post)
* [I.8: 优先使用 `Ensures()` 来表达后置条件](#Ri-ensures)
* [I.9: 如果接口是模板，使用concepts来文档化它的参数](#Ri-concepts)
* [I.10: 使用异常来表示执行所需任务失败](#Ri-except)
* [I.11: 永远不要使用原始指针(`T*`)和引用(`T&`)来转移所有权](#Ri-raw)
* [I.12: 声明不能为null的指针为`not_null`](#Ri-nullptr)
* [I.13: 不要以单个指针的形式传递数组](#Ri-array)
* [I.22: 避免全局对象的复杂初始化](#Ri-global-init)
* [I.23: 函数的参数个数要少 ](#Ri-nargs)
* [I.24: 避免相邻不相关参数的类型相同](#Ri-unrelated)
* [I.25: 优先使用抽象类，而非类层次结构](#Ri-abstract)
* [I.26: 如果想要跨编译器折ABI，那么使用C-style的子集](#Ri-abi)
* [I.27: 为稳定的库ABI，考虑Pimpl](#Ri-pimpl)
* [I.30: 封装违反规则的行为](#Ri-encapsulate)

**也参见**:

* [F: 函数](#S-functions)
* [C.concrete: 具体的类型](#SS-concrete)
* [C.hier: 类层次结构](#SS-hier)
* [C.over: 重载和操作符重载](#SS-overload)
* [C.con: 容器和其他资源句柄](#SS-containers)
* [E: 错误处理](#S-errors)
* [T: 模板和泛型编程](#S-templates)

### <a name="Ri-explicit"></a>I.1: 使接口明确

##### 原因

正确性，没有被接口说明的假设容易被忽略，且难以测试。

##### 糟糕的示例

隐式地通过全局(命名空间)变量(调用模式)来控制函数的行为可能会造成混淆，例如:

    int round(double d)
    {
        return (round_up) ? ceil(d) : d;    // 不要这样做: "不可见的" 依赖
    }

对于调用者来说，`round(7.2)`的两个调用可能会产生不同的结果这一点并不明显。

##### 例外

有时我们通过一组环境变量来控制一组操作的细节，如正常输出和冗长(verbose)输出或者调式的和优化的，使用非本地的控制(变量)存在潜在的混乱，但控制”具有固定语义的“实现细节除外。

##### 糟糕的示例

非本地变量的报告（如，`errno`）很容易被忽略，例如：

    // 不要这样做: 没有测试printf的返回值
    fprintf(connection, "logging: %d %d %d\n", x, y, s);

如果`connection`失败从而导致没输出日志产生怎么办？参见I.???.

**可选方案**: 抛出异常，异常是不可以被忽略的。

**可选方案**: 避免使用非本地变量或隐式状态在接口间传递信息，注意非`const`的成员函数使用它们的对象状态（成员变量）来向其它成员函数传递信息。

**可选方案**: 一个接口应该是一个函数或一组函数，函数可以是模板函数，函数集合可以是类或者模板类。

##### 实施

* (简单) 函数的控制流不应该基于命名空间(`namespace`)范围的变量来做决定。
* (简单) 函数不应该更改命名空间(`namespace`)范围的变量。

### <a name="Ri-global"></a>I.2: 避免非`const`的全局变量

##### 原因

非`const`的全局变量隐藏了依赖关系，依赖了不可预测的变化。

##### 示例

    struct Data {
        // ... 很多东西 ...
    } data;            // 非const的data

    void compute()     // 不要这样做
    {
        // ... 使用 data ...
    }

    void output()     // 不要这样做
    {
        // ... 使用 data ...
    }

还有谁可以修改`data`?

##### 注意

全局常量是有用的。

##### 注意

对全局变量规则适用于名称空间(`namespace`)范围变量。

**可选方案**: 如果你想使用全局(更加常用的namespace)数据来避免拷贝操作，考虑使用`const`的引用来传递数据对象，另一个方案是将数据和操作定义一个对象的状态(成员变量)和成员函数。

**警告**: 谨防数据竞争(data race)：如果一个线程可以访问非局部数据(或数据以引用的方式传递)，而另一个线程同时执行被调用的函数，那么就会存在数据竞争。每一个可变数据的指针或引用都是一个潜在的数据竞争。

##### Note

不可变数据不存在数据竞争问题。

**参考**: 参见 [rules for calling functions](#SS-call).

##### Note

规则是”避免“而不是”不要使用“，也存在（不多）的异常情况，如`cin`， `cout`和`cerr`。

##### 实施

(简单) 报告所有在命名空间范围内的非`const`变量。

### <a name="Ri-singleton"></a>I.3: 避免单例

##### 原因

单例就是伪装的复杂全局对象。

##### 示例

    class Singleton {
        // ... 很多工作来保证只有一个单例对象被创建和正确的初始化。
    };

有非常多不同的单例变体，这只是总是的一部分。

##### Note

如果你不想更改一个全局对象，将它声明为`const`或`constexpr`。

##### 例外情况

您可以使用最简单的“单例”(非常简单，以至于通常不认为它是单例)，以便在第一次使用时进行初始化：
    
    X& myX()
    {
        static X my_x {3};
        return my_x;
    }

这是解决初始化顺序相关问题的最有效方法之一。在多线程环境中，静态对象的初始化不会引入竞态条件(除非您不小心从共享对象的构造函数中访问它)。

注意，局部`static`对象的初始化并不意味着有竞态条件，但是，当`X`的析构中包含需要同步的操作时，我们必要使用一种不那么简单的方案，例如：

    X& myX()
    {
        static auto p = new X {3};
        return *p;  // 潜在的泄漏
    }

现在必须有人适当地以线程安全的方式`delete`该对象，那是易于出错的，所以我们不使用这种技巧，除非有下面这些情况：

* `myX`是多线程的代码
* `X`对象需要被销毁 (等等，因为它释放资源)
* `X`的析构代码需要同步

如果你和许多人一样将单例定义为只创建一个对象的类，那么像`myX`这样的函数就不算是单例，这种技巧也不是避免使用单例这一规则的例外情况。

##### 实施

通常来说很难。

* 寻找名字中包含`singleton`的类。
* 寻找只创建了一个对象的类（通过对象计数或检测构造函数）。
* 如果类X中有一个公有静态函数，该函数中有一个类型为X的静态局部(函数)对象，并返回了该指向对象的的引用或指针时，禁用该函数。

### <a name="Ri-typed"></a>I.4: 使接口精确且强类型

##### Reason

类型是最简单和最好的文档，其明确的含义提高了可读性，亦可在编译时进行检查。此外，精确类型的代码通常优化得更好。

##### 示例, 不要这样用

Consider:

    void pass(void* data);    // 弱类型和未限定的void*是可疑的

调用者不确定什么类型是允许的，由于`const`没有指定，所以也不确定data是否可变。注意，所有的指针类型都可以被隐藏转换成`void*`，所以调用者很容易提供这个值（译注：太过随意）。

被调用者必须要`static_cast`到未确认的类型来使用data（译注：通常不能直接操作void*类型），这是易于出错且啰唆的。

在C++中仅仅使用`const void*`来传递数据也是难以捉摸的，考虑使用`variant`或者指针基类的指针。

**可先方案**: 通常，模板参数可以通过将其转换成`T*`或`T&`来消除`void*`，在泛型代码中`T`可以通用或概念受限的模板参数。

##### 糟糕的示例

考虑:

    draw_rect(100, 200, 100, 500); // 这些数字指什么？

    draw_rect(p.x, p.y, 10, 20); // 10和20的单位是什么?

很明显，调用者正在描述一个矩形，但它们关联的部分并不明显，并且`int`可以携带任意形式的信息，包括许多单位的值，所以我们需要猜测4个`int`的含义，前面两个很有可能是`x`和`y`坐标，但后面两个是什么呢？

虽然注释和参数名字是有帮助的，但是我们可以显示地这样声明：

    void draw_rectangle(Point top_left, Point bottom_right);
    void draw_rectangle(Point top_left, Size height_width);

    draw_rectangle(p, Point{10, 20});  // 两个角落（译注：左上，右下）
    draw_rectangle(p, Size{10, 20});   // 一个角落和一个(height, width)对

显然，我们无法使用静态类型系统来捕获所有的错误（例如，按照惯例(名称和注释)左上角应是第一个参数）。

##### 糟糕的例子

考虑如下:

    set_settings(true, false, 42); //数字指什么?

参数类型及其值没有表达出正在指定的设置或这些值的含义。

(下面)这个设计更加显示、安全且易读：

    alarm_settings s{};
    s.enabled = true;
    s.displayMode = alarm_settings::mode::spinning_light;
    s.frequency = alarm_settings::every_10_seconds;
    set_settings(s);

考虑使用枚举(enum)来表示一组布尔值(boolean)，这是表示一组布尔值的模式。

    enable_lamp_options(lamp_option::on | lamp_option::animate_state_transitions);

##### 糟糕的示例

这下面这个示例中，`time_to_blink`的含义不并能直观地从接口中看出，秒？毫秒？

    void blink_led(int time_to_blink) // 差 -- 单位是有歧义的
    {
        // ...
        // 使用time_to_blink做一些事
        // ...
    }

    void use()
    {
        blink_led(2);
    }

##### 好的示例

`std::chrono::duration`(C++11)有助于显示表达连续时间的单位。

    void blink_led(milliseconds time_to_blink) // 好 -- 单位明确
    {
        // ...
        // 使用time_to_blink做一些事
        // ...
    }

    void use()
    {
        blink_led(1500ms);
    }

该函数也可以这样写来接收任意的时间单位：

    template<class rep, class period>
    void blink_led(duration<rep, period> time_to_blink) // 好 -- 接收任意单位
    {
        // 假定毫秒是相关的最小单位
        auto milliseconds_to_blink = duration_cast<milliseconds>(time_to_blink);
        // ...
        // 使用milliseconds_to_blink来做事
        // ...
    }

    void use()
    {
        blink_led(2s);
        blink_led(1500ms);
    }

##### Enforcement

* (Simple) Report the use of `void*` as a parameter or return type.
* (Simple) Report the use of more than one `bool` parameter.
* (Hard to do well) Look for functions that use too many primitive type arguments.

* (简单) 报告所有使用`void*`作为参数或返回值的函数。
* (简单) 报告不止一个`bool`类型参数的函数。
* (很难做到很好) 寻找使用太多基本类型参数的函数

### <a name="Ri-pre"></a>I.5: 声明先决条件 (如果有的话)

##### 原因

参数的含义可能会限制它们在被调用对象中的正确使用。

##### 示例

考虑:

    double sqrt(double x);

这里`x`必须是非负的，类型系统不能(简单而自然地)表达这一点，因此我们必须使用其他方法，例如:

    double sqrt(double x); // x 必须是非负的

一些先决条件可以表示为断言(assert)，例如:

    double sqrt(double x) { Expects(x >= 0); /* ... */ }

理想情况下，`expected (x >= 0)`应该是`sqrt()`接口的一部分，但这并不容易做到，现在，我们将它放在定义(函数体)中。

**参考**: `Expects()`在[GSL](#S-gsl)的描述.

##### Note

优先使用需求的正式规范，如`Expects(p);`，如是不可行，则在注释中注明，如`// 序列[p:q)使用<进行排序`。

##### Note

大多数类成员函数都有一个让某些类的不变式成立的先决条件，该不变式由构造函数建立，并且必须由从类外部调用的每个成员函数在退出时重新建立，我们不需要在每个成员函数都提到它。

##### 实施

不可强行的

**参见**: 传递指针的规则 ???

### <a name="Ri-expects"></a>I.6: 使用`Expects()`表达先决条件

##### 原因

明确条件是先决条件，并允许使用(自动化)工具进行检测。

##### 示例

    int area(int height, int width)
    {
        Expects(height > 0 && width > 0);            // 好的
        if (height <= 0 || width <= 0) my_error();   // 晦涩的，不明确的
        // ...
    }

##### Note

先决条件可以用多种方式声明，包括注释、`if`语句和`assert()`等，但这可能使它们难以与普通代码区分，难以更新，难以被工具操作，并且可能具有错误的语义(您是否总是希望能在debug模式下中止(abort)，并且在产品release时不做检查?)

##### Note

先决条件应该是接口的一部分，而不是实现的一部分，但我们还没有语言特性来做到这一点，一旦语言支持这一点(例如，参见[合同建议](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0380r1.pdf)，我们将采用先决条件、后置条件和断言的标准版本。

##### Note

`Expects()`也可以用来检查算法中间的条件。

##### Note

使用`unsigned`也不是回避这个问题[保证值是非负的](#Res-nonnegative)的好方法。

##### 实施

（不可强制的）寻找各种可被断言(assert)的先决条件是不可行的，在缺乏语言特性支持的情况下，对那些容易识别(`assert()`)的对象发出警告也是有问题的。

### <a name="Ri-post"></a>I.7: 声明后置条件

##### 原因

检测对结果的误解，并可能捕获错误的实现。

##### 糟糕的示例

考虑:

    int area(int height, int width) { return height * width; }  // 糟糕的

 我们(无意识地)遗漏了先决条件，所以`height`和`width`必须为正值就不那么明显。我们同样遗漏了后置条件，所以对于超过最大整数范围的区域，算法(`height * width`)的错误并不那么显而易见，溢出（对于乘法）是有可能发生的，考虑使用：

    int area(int height, int width)
    {
        auto res = height * width;
        Ensures(res > 0);
        return res;
    }

##### 糟糕的示例

考虑一个著名的安全漏洞：

    void f()    // 有问题的做法
    {
        char buffer[MAX];
        // ...
        memset(buffer, 0, sizeof(buffer));
    }

没有后置条件声明buffer应该被清除，优化器(译注：编译器可能会)消除了明显冗余的`memset()`调用:

    void f()    // 更好的做法
    {
        char buffer[MAX];
        // ...
        memset(buffer, 0, sizeof(buffer));
        Ensures(buffer[0] == 0);
    }

##### Note

后置条件通常在表明函数目的的注释中被非正式地声明，可以使用`ensure()`使其更系统化、更可见和更具可检查性。

##### Note

当后置条件关联的内容并没有直接反映在返回结果中（如数据结构的状态）时，后置条件尤其重要。

##### 示例

考虑一个函数在操作`Record`时使用`mutex`来避免竞态条件：

    mutex m;

    void manipulate(Record& r)    // 不要这样做
    {
        m.lock();
        // ... no m.unlock() ...
    }

这里，我们“忘记”声明`mutex`应该被释放，所以我们不知道没有释放`mutex`到底是一个bug还是一个功能(feature)。声明后置条件会让函数更加清晰：

    void manipulate(Record& r)    // 后置条件：当退出时m应该被unlock
    {
        m.lock();
        // ... no m.unlock() ...
    }

现在这个bug是明显的（但只有当人阅读注释时），更好的方法是使用[RAII](#Rr-raii)来确保后置条件(“锁必须被释放”)在代码中一定会执行：

    void manipulate(Record& r)    // 最好的
    {
        lock_guard<mutex> _ {m};
        // ...
    }

##### Note

理想情况下，后置条件应声明在接口或声明，以便用户可以方便地看到。只有与用户相关的后置条件才声明在接口中，仅与内部状态相关的后置条件则声明在定义或实现中。

##### 实施

(不可强制执行)这是一个哲学意义上指引，在一般情况下无法直接检查，但许多工具链中存在特定领域的检查器(如锁持有检查器)。

### <a name="Ri-ensures"></a>I.8: 使用`Ensures()`来表达后置条件

##### 原因

明确一个条件是后置条件，并可使用工具进行检查。

##### 示例

    void f()
    {
        char buffer[MAX];
        // ...
        memset(buffer, 0, MAX);
        Ensures(buffer[0] == 0);
    }

##### Note

后置条件可以使用多种方式进行声明，包括注释、`if`语句和`assert()`等，但这可能使它们难以与普通代码区分，难以更新，难以被工具操作，并且可能具有错误的语义。

**可行方案**: ”资源必须被释放“类型的后置条件可以使用[RAII](#Rr-raii)来更好的表达。

##### Note

理想情况下，`Ensures`应该是接口的一部分，但这是不容易实现的，现在我们把它放在定义(函数体)中。一旦语言支持这一点(例如，参见[合同建议](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0380r1.pdf)，我们将采用先决条件、后置条件和断言的标准版本。

##### 实施

（不可强制执行）寻找各种可被断言(assert)的后置条件是不可行的，在缺乏语言特性支持的情况下，对那些容易识别(`assert()`)的对象发出警告也是有问题的。

### <a name="Ri-concepts"></a>I.9: 如果接口是模板，则使用concept文档化其参数

##### 原因

精确地指定接口，(不远的)将来在编译时进行检查。

##### 示例

使用ISO Concepts TS(Technical Specifications)风格的需求规范(译注：Concepts并未出现在C++17的标准中，而只是作为技术规范来补充C++17)，例如

    template<typename Iter, typename Val>
    // requires InputIterator<Iter> && EqualityComparable<ValueType<Iter>>, Val>
    Iter find(Iter first, Iter last, Val v)
    {
        // ...
    }

##### Note

不久的将来(可能在2018年)，一旦`//`被移除，大部分的编译都具备检查`requirers`的能力。`Concepts`在GCC 6.1或更新的版本中已经支持(译注：公开的资料显示，仅在这一个公开发行的GCC版本中存在过)。

**参见**: [泛型编程](#SS-GP) and [concepts](#SS-concepts).

##### 实施

还不能强制执行，该语言工具（Concepts）只还在规范中，当工具可用时，如果任何非可变参数不受某个概念(Concept)的约束则发出警告。

### <a name="Ri-except"></a>I.10: 使用异常来表示执行所需任务的失败

##### 原因

不应该忽略错误，因为这会使系统或计算处于未定义(或意外)的状态，这也是错误的主要来源。

##### 示例

    int printf(const char* ...);    // 糟糕：当输出失败时返回负值

    template <class F, class ...Args>
    // 好的：当不能开始一个新线程时抛出system_error
    explicit thread(F&& f, Args&&... args);

##### Note

什么是错误？

一个错误意味着函数不能达到它所宣传的目的(包括建立后置条件)，忽略错误的调用代码可能导致错误的结果或未定义的系统状态。例如，不能连接到远程服务器本身并不是一个错误：服务器可以出于各种原因拒绝连接，因此自然会返回调用者应该始终检查的结果；但是，如果连接失败被认为是错误，则失败应该引发异常。

##### 例外

许多传统的接口函数(例如UNIX信号处理程序)使用错误代码(例如`errno`)来报告真正的状态，而不是错误。没有很好的替代方法来使用这样的函数，因此调用这些函数并不违反规则。

##### 替代方法

If you can't use exceptions (e.g., because your code is full of old-style raw-pointer use or because there are hard-real-time constraints), consider using a style that returns a pair of values:

如果您不能使用异常(例如，因为代码充满了旧式的raw指针使用，或者因为存在硬实时(hard-real-time)约束)，请考虑使用返回一对值的方式:

    int val;
    int error_code;
    tie(val, error_code) = do_something(); // 译注：tie是C++11的用法
    if (error_code) {
        // ... 处理错误或退出 ...
    }
    // ... 使用 val ...

不幸的是，这种风格导致了未初始化的变量（译注：`int val;`会导致val没有被初始化），处理这个问题的工具[结构化绑定](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0144r1.pdf)将在C++17中提供（译注：C++17已经支持）。

    auto [val, error_code] = do_something();
    if (error_code) {
        // ... 处理错误或退出 ...
    }
    // ... 使用 val ...

##### Note

我们不认为“性能”是不使用异常的正当理由：

* 通常，显式的错误检查和处理也会消耗与异常处理同样多的时间和空间。
* 通常，干净的代码在有异常的情况下会产生更好的性能（简化和优化程序中路径的跟踪）。
* 性能关键代码的一个好规则是将检查移到代码的关键部分之外([checking](#Rper-checking))。
* 从长远来看，更规范的代码会得到更好的优化。
* 在做出性能声明之前一定要仔细[度量](#Rper-measure)。

**另请参阅**: [I.5](#Ri-pre) 和 [I.7](#Ri-post) 来报告违反先决条件和后置条件的情况

##### 实施

* (不可强制执行的) 这只是一个哲学意义上的指导，不具直接检查的可行性。
* 寻找 `errno`。

### <a name="Ri-raw"></a>I.11: 永远不要使用原始指针(`T*`)或引用(`T&`)来转移所有权

##### 原因

如果对调用者或被调用者是否拥有对象有任何疑问，就会发生泄漏或过早析构。

##### 示例

考虑:

    X* compute(args)    // 不要这样做
    {
        X* res = new X{};
        // ...
        return res;
    }

谁来删除返回的`X`？当`compute`返回一个引用时，则更难发现问题。考虑按值返回结果（如果结果很大，使用move语义）：

    vector<double> compute(args)  // good
    {
        vector<double> res(10000);
        // ...
        return res;
    }

**可选方法**: [传递所有权](#Rr-smartptrparam) 使用"智能指针"，如`unique_ptr`(独占所有权)和`shared_ptr`(共享所有权)，然而，这并不会比返回对象本身更优雅和高效，所以只有在需要引用语义时才使用智能指针。

**可选方法**: 有时，由于ABI的兼容性要求和资源缺失，导致更旧代码无法修改，在这种情况下，使用[指南支持库](#S-gsl)中的`owner`标记拥有指针：

    owner<X*> compute(args)    // 现在很清楚，所有权已经转移
    {
        owner<X*> res = new X{};
        // ...
        return res;
    }

这告诉分析工具`res`是所有者，也就是说，它的值必须被`delete`或转移到另一个所有者，就像这里的`return`所做的那样。`owner`在资源句柄的实现中有也类似应用。

##### Note

作为原始指针(或迭代器)传递的每个对象都假定为调用者所拥有，因此它的生命周期由调用者处理。从另一个角度来看：与指针传递API相比，所有权转移API相对较少，因此默认为“无所有权转移”。

**另请参阅**: [参数传递](#Rf-conventional)、 [智能指针参数的使用](#Rr-smartptrparam) 和 [按值返回](#Rf-value-return).


##### 实施

* (简单) 警告`delete`非`owner<T>`的原始指针，建议使用标准库资源句柄或使用`owner<T>`。
* (简单) 每条代码路径上`reset`或显式`delete`一个`owner`指针时，警告失败。
* (简单) 如果`new`的返回值或带`owner`返回值的函数调用被分配给原始指针或非`owner`引用，则发出警告。

### <a name="Ri-nullptr"></a>I.12: 声明一个非空指针为`not_null`

##### 原因

帮助避免`nullptr`指针的解引用错误，通过避免对`nullptr`的冗余检查来提高性能。

##### 示例

    int length(const char* p);            // 不清楚length(nullptr)是否合法

    length(nullptr);                      // OK?

    int length(not_null<const char*> p);  // better: 我们假定p不能为nullptr

    int length(const char* p);            // 我们必须假定p可以为nullptr

通过在源代码中声明意图，实现者和工具可以提供更好的诊断，比如通过静态分析找到某些错误类，以及执行优化，比如删除分支和空测试。

##### Note

`not_null` 定义在 [指南支持库](#S-gsl).

##### Note

指向`char`的指针指向C风格的字符串(以`\0`结尾的字符串)这一个假设也是隐式的，这是困惑和错误的一个潜在根源。优先使用`czstring`，而不是`const char*`。

    // 我们假设p不能为nullptr
    // 我们假设p指向以0结尾的字符数组
    int length(not_null<zstring> p);

注意：`length()`只是`std::strlen()`的伪装。

##### 实施

* (简单) ((基础))，如果函数在所有控制流路径上使用指针参数之前都检查`nullptr`，则警告应当声明为`not_null`。
* (复杂) 如果函数在所有返回路径上都能保证返回的指针不为`nullptr`，则警告返回类型就声明为`not_null`。

### <a name="Ri-array"></a>I.13: 不要将一个数组作为单个指针进行传递

##### 原因

 (指针, 大小)风格的接口是易于出错的，同样，单独的指针(指向数组)必须依赖一些惯例才能让调用者知道它的大小(译注：数组元素个数)。

##### 示例

考虑:

    void copy_n(const T* p, T* q, int n); // 将[p:p+n) 拷贝到 [q:q+n)

如果`q`指向的数组中少于`n`个元素怎么办？然后，我们就可能覆写了一些不相关的内存；如果`p`指向的数组中少于`n`个元素怎么办？然后，我们就可能读了一些不相关的内存。要么是一个未定义的行为，要么就是一个潜在的严重bug


##### 可选方法

考虑使用显示的`span`:

    void copy(span<const T> r, span<T> r2); // copy r to r2

##### 糟糕的示例

考虑:

    void draw(Shape* p, int n);  // 糟糕的接口; 糟糕的代码
    Circle arr[10];
    // ...
    draw(arr, 10);

传递`10`给参数`n`可能是错误的：最常见的习惯是假设`[0:n)`，但并没有地方说明这一点。更糟糕的是`draw()`调用的编译：有一个隐式从数组到指针的转换(数组衰退)，以及另一个从`Circle`到`Shape`的转换。`draw()`不能安全地遍历该数组：无法得知数组元素的数量。

**可选方法**: 使用一个支持类来确保元素的数量是正确的，并防止危险的隐式转换，例如:

    void draw2(span<Circle>);
    Circle arr[10];
    // ...
    draw2(span<Circle>(arr));  // 推导出元素的个数
    draw2(arr);    // 推导元素类型和数组大小

    void draw3(span<Shape>);
    draw3(arr);    // 错误：不能将Circle[10] 转换到 span<Shape>

`draw2()`将同样的数量信息传递给了`draw()`，它使得该信息就是`Circle`的范围这一假定明确起来，参见 ???。

##### 例外

使用`zstring`和`czstring`来表示以`'\0'`结尾的C风格字符串，当这样做时，使用 [GSL](#GSL)的`string_span`来阻止错误发生。

##### 实施

* (简单) ((边界)) 对所有依赖隐式转换数组到指针的表达式进行警告，zstring/czstring指针类型例外。
* (简单) ((边界)) 对于指针类型表达式上的任何算术运算，如果运算结果为指针类型值，则发出警告，zstring/czstring指针类型例外。

### <a name="Ri-global-init"></a>I.22: 避免全局对象的复杂初始化

##### 原因

复杂的初始化可能会导致未定义的执行顺序。

##### 示例

    // file1.c

    extern const X x;

    const Y y = f(x);   // read x; write y

    // file2.c

    extern const Y y;

    const X x = g(y);   // read y; write x

由于`x`和`y`在不同的翻译单元，`f()`和`g()`的调用顺序是不确定的，其中一个可能会访问一个未初始化的`const`，这也显示了全局(命名空间)对象的"初始化顺序"问题不局限于全局*变量*(译注:全局常量也会存在初始华顺序问题)。

##### Note

初始化顺序问题在并发代码中变得尤其困难，通常最好是完全避免使用全局(命名空间)对象。

##### 实施

* 标出没有调用`constexpr`函数的全局对象的初始化。
* 标出访问`extern`对象的全局对象的初始化。

### <a name="Ri-nargs"></a>I.23: 保持较少的函数参数

##### 原因

过多的参数容易引起迷惑，与其它方法相比，传递太多参数的成本通常更高。

##### 讨论

函数传递太多参数的两个最常见原因是：

1. *缺乏抽象*

    由于缺乏抽象，所以复合值(compound value)作为独立的参数进行传递，而不是作为一个强制不变的对象，这(复合值)不仅仅是展开参数列表，也会由于不再受到强制不变式的保护而导致错误。

2. *违反“函数单一职责”*

    企图做超过一个工作的函数可能应当被重构。

##### 示例

标准库的`merge()`是我们能够轻松处理的极限(译注：参数个数太多):

    template<class InputIterator1, class InputIterator2, class OutputIterator, class Compare>
    OutputIterator merge(InputIterator1 first1, InputIterator1 last1,
                         InputIterator2 first2, InputIterator2 last2,
                         OutputIterator result, Compare comp);

注意，这是由于上述的问题1 -- *缺乏抽象*，STL传递了迭代器对(iterator pair)(未封装的组件值)，而不是传递范围(range)(抽象)。

这里，我们有4个模板参数和6个函数参数，为了简化最常见和最简单的用法，比较参数默认为`<`:

    template<class InputIterator1, class InputIterator2, class OutputIterator>
    OutputIterator merge(InputIterator1 first1, InputIterator1 last1,
                         InputIterator2 first2, InputIterator2 last2,
                         OutputIterator result);

这并没有减少总的复杂性，但是它减少了呈现给许多用户的表面(接口)复杂性，为了真正减少参数的数量，我们需要将参数捆绑到更高层次的抽象中:

    template<class InputRange1, class InputRange2, class OutputIterator>
    OutputIterator merge(InputRange1 r1, InputRange2 r2, OutputIterator result);

将参数组合到"boundle"中是减少参数数量的通用技术，并增加了对其进行检查的机会。

Alternatively, we could use concepts (as defined by the ISO TS) to define the notion of three types that must be usable for merging:

或者，我们可以使用概念(concepts，由ISO技术规范定义)来定义三种类型(In1, In2, Out)的概念，这些类型必须可用于合并:

    Mergeable{In1, In2, Out}
    OutputIterator merge(In1 r1, In2 r2, Out result);

##### 示例

安全配置文件(safety profile)推荐使用

    void f(gsl::span<int> some_ints);              // GOOD: safe, bounds-checked

来替换

    void f(int* some_ints, int some_ints_length);  // BAD: C style, unsafe

这里，使用抽象能带来安全性和鲁棒性的收益，自然也能减少参数的数量。    

##### Note

多少参数才算太多？尝试不超过四个参数。有些函数最好用四个单独的参数来表示，但不是很多。

**可选方法**：使用更好的抽象：将参数分组到有意义的对象中，然后以传值来引用的方式来传递这些对象。

**可选方法**：使用默认参数或重载的方式来让最常见形式的调用只有很少的参数。

##### 实施

* 发出警告，如果函数声明了两个同种类型的迭代器(含指针)，而不是范围(range)或视图(view)。
* (无法强制) 这只是一种哲学意义上的指导方针，并不具有直接检查的可行性。

### <a name="Ri-unrelated"></a>I.24: 避免邻近不相关的同类型参数

##### 原因

邻近同类型的参数很容易被错误地交换。

##### 糟糕的例子

考虑:

    void copy_n(T* p, T* q, int n);  // copy from [p:p + n) to [q:q + n)

这是一个K&R C风格接口的可恶变种， 容易弄反"to"和"from"两个参数。在`from`上使用`const`:

    void copy_n(const T* p, T* q, int n);  // copy from [p:p + n) to [q:q + n)

##### 例外

如果两个参数的顺序无关紧要，就没有什么问题：

    int max(int a, int b);

##### 可选

不要以指针的方式传递数组，传递代表范围的对象（如`span`）:

    void copy_n(span<const T> p, span<T> q);  // copy from p to q

##### 可选

定义一个`struct`作为参数类型，并相应地为这些参数命名字段:

    struct SystemParams {
        string config_file;
        string output_path;
        seconds timeout;
    };
    void initialize(SystemParams p);

这使得将来的读者能够清楚地调用这些参数，因为这些参数通常是在调用站点(call site)上按名称填充的。

##### 实施

(简单) 发出警告，如果两个相邻的参数具有相同的类型。

### <a name="Ri-abstract"></a>I.25: 侧重使用抽象类而不是类层次来作为接口

##### 原因

抽象类比具有状态的基类可能更稳定。

##### 糟糕的例子

你只知道`Shape`会在某些地方出现:-)

    class Shape {  // 糟糕: 接口类和数据一起加载
    public:
        Point center() const { return c; }
        virtual void draw() const;
        virtual void rotate(int);
        // ...
    private:
        Point c;
        vector<Point> outline;
        Color col;
    };

这将迫使每个派生类都要计算中心点--即使是不重要的(译注：原文non-trivial-意义重要大，可能是笔误)，并且中心从未使用过。并非使用的`Shape`都有`Color`，并且很多`Shape`最好的表示并没有使用`Point`序列定义的轮廓，抽象类的出现并不鼓励用户写出这样的类：

    class Shape {    // better: Shape是纯接口
    public:
        virtual Point center() const = 0;   // 纯虚函数
        virtual void draw() const = 0;
        virtual void rotate(int) = 0;
        // ...
        // ... 没有数据成员 ...
        // ...
        virtual ~Shape() = default;
    };

##### 实施

(简单) 如果类`C`的指针/引用被赋值给具有数据成员的`C`的基类，则发出警告。

### <a name="Ri-abi"></a>I.26: 如果想要跨编译器的ABI，则使用C风格的子集

##### 原因

不同的编译实现了不同的类的二进制布局、异常处理、函数名字(name mangling)和其它的一些细节。

##### 例外

您可以使用精心选择的一些高级c++类型来精心设计接口，参见 ???。

##### 例外

常见的ABI正在一些平台上出现，将您从更严格的限制中解放出来。

##### 注意

如果使用单一编译器，则可以在接口中使用完整的C++，这可能需要在升级到新的编译器版本后重新编译。

##### 实施

(不可强制)很难可靠地确定接口在何处构成ABI的一部分。

### <a name="Ri-pimpl"></a>I.27: 使用Pimpl来达到稳定的ABI库

##### 原因

由于私有数据成员影响类布局，而私有成员函数参与重载解析，因此对这些实现细节的更改需要类的所有用户重新编译。持有指向实现指针(Pimpl)的非多态接口类可以将类的用户与其实现中的更改隔离开来，代价是不再直接(译注：多了一层封装)。

##### Example

接口 (widget.h)

    class widget {
        class impl;
        std::unique_ptr<impl> pimpl;
    public:
        void draw(); // 转发到实现的公共API
        widget(int); // 在实现文件中进行定义
        ~widget();   // 在实现文件中进行定义，在那里"impl"是一个完整的类型
        widget(widget&&) = default;
        widget(const widget&) = delete;
        widget& operator=(widget&&); // 在实现文件中进行定义
        widget& operator=(const widget&) = delete;
    };


实现 (widget.cpp)

    class widget::impl {
        int n; // 私有数据
    public:
        void draw(const widget& w) { /* ... */ }
        impl(int n) : n(n) {}
    };
    void widget::draw() { pimpl->draw(*this); }
    widget::widget(int n) : pimpl{std::make_unique<impl>(n)} {}
    widget::~widget() = default;
    widget& widget::operator=(widget&&) = default;

##### Notes

参见[GOTW #100](https://herbsutter.com/gotw/_100/)和[cppreference](http://en.cppreference.com/w/cpp/language/pimpl)了解与此习惯用法相关的权衡和其他实现细节。

##### 实施

(不可强制)很难可靠地确定接口在何处构成ABI的一部分。

### <a name="Ri-encapsulate"></a>I.30: 封装违反规则的行为

##### 原因

为了让代码操持简洁和安全。有时，丑陋、不安全或易于出错的技术对逻辑或性能而言是必不可少的，如果是这样，将它们控制在局部，而不是"感染"接口，以便更多的程序员能理解这些代码的精髓。如果可以的话，实现的复杂性应该不要通过接口泄漏给用户代码。

##### 示例

考虑一个依赖于某些形式输入(如`main`的参数)的程序，输入可能是一个文件、命令行、或标准输入，我们可能会这样写：

    bool owned;
    owner<istream*> inp;
    switch (source) {
    case std_in:        owned = false; inp = &cin;                       break;
    case command_line:  owned = true;  inp = new istringstream{argv[2]}; break;
    case file:          owned = true;  inp = new ifstream{argv[2]};      break;
    }
    istream& in = *inp;

违反了规则 [未初始化的变量](#Res-always)、[忽略所有权](#Ri-raw)和[魔术常量(magic constant)](#Res-magic)。特别是，一些人需要记住在某些地方写下：

    if (owned) delete inp;

对于这个特殊的例子，我们可以使用`unique_ptr`，配合一个可以对`cin`不做任何处理的删除器(deleter)。但这对新手(经常会遇到这种问题)程序员来说太复杂了，但这也是一个更普遍的问题的例子 -- 我们想要静态(这里是所有权)需求的属性在运行时很少被处理。最常见、最常见和最安全的示例可以静态处理，因此我们不想给这些示例增加成本和复杂性，但是我们也必须要处理不怎么常见，不太安全以及需求更加昂贵的例子，这些例子的讨论参见[[Str15]](http://www.stroustrup.com/resource-model.pdf)。

所以，我们这样写一个类：

    class Istream { [[gsl::suppress(lifetime)]]
    public:
        enum Opt { from_line = 1 };
        Istream() { }
        Istream(zstring p) :owned{true}, inp{new ifstream{p}} {}            // read from file
        Istream(zstring p, Opt) :owned{true}, inp{new istringstream{p}} {}  // read from command line
        ~Istream() { if (owned) delete inp; }
        operator istream& () { return *inp; }
    private:
        bool owned = false;
        istream* inp = &cin;
    };

现在，`istream`所有权的动态本质已经被封装，或许，在实际代码中将会添加一些检查潜在错误的功能。

##### 实施

* 很难，很难决定什么违反规则的代码是必要的
* Flag rule suppression that enable rule-violations to cross interfaces

# <a name="S-functions"></a>F: 函数

函数指将系统从一个(一致)状态带到下一个(一致状态)的操作或计算，它是程序的基本构件。

应该可以对函数进行有意义的命名，指定参数的需求，以及明确指明参数和返回值之间的关系。函数的实现不是一个技术规范，尝试思考一个函数要做什么，以及怎么做。函数在大多数接口在都是最重要的部分，参见接口规则部分。

函数规则总结：

函数定义规则：

* [F.1: 在函数名字中表达函数的意图](#Rf-package)
* [F.2: 一个函数应该只执行一个逻辑操作](#Rf-logical)
* [F.3: 保持函数简短](#Rf-single)
* [F.4: If a function may have to be evaluated at compile time, declare it `constexpr`](#Rf-constexpr)
* [F.5: If a function is very small and time-critical, declare it inline](#Rf-inline)
* [F.6: If your function may not throw, declare it `noexcept`](#Rf-noexcept)
* [F.7: For general use, take `T*` or `T&` arguments rather than smart pointers](#Rf-smart)
* [F.8: Prefer pure functions](#Rf-pure)
* [F.9: Unused parameters should be unnamed](#Rf-unused)

Parameter passing expression rules:
参数表达

* [F.15: Prefer simple and conventional ways of passing information](#Rf-conventional)
* [F.16: For "in" parameters, pass cheaply-copied types by value and others by reference to `const`](#Rf-in)
* [F.17: For "in-out" parameters, pass by reference to non-`const`](#Rf-inout)
* [F.18: For "will-move-from" parameters, pass by `X&&` and `std::move` the parameter](#Rf-consume)
* [F.19: For "forward" parameters, pass by `TP&&` and only `std::forward` the parameter](#Rf-forward)
* [F.20: For "out" output values, prefer return values to output parameters](#Rf-out)
* [F.21: To return multiple "out" values, prefer returning a struct or tuple](#Rf-out-multi)
* [F.60: Prefer `T*` over `T&` when "no argument" is a valid option](#Rf-ptr-ref)

参数传递表达式规则：

* [F.22: Use `T*` or `owner<T*>` to designate a single object](#Rf-ptr)
* [F.23: Use a `not_null<T>` to indicate that "null" is not a valid value](#Rf-nullptr)
* [F.24: Use a `span<T>` or a `span_p<T>` to designate a half-open sequence](#Rf-range)
* [F.25: Use a `zstring` or a `not_null<zstring>` to designate a C-style string](#Rf-zstring)
* [F.26: Use a `unique_ptr<T>` to transfer ownership where a pointer is needed](#Rf-unique_ptr)
* [F.27: Use a `shared_ptr<T>` to share ownership](#Rf-shared_ptr)

<a name="Rf-value-return"></a>值返回语义规则：

* [F.42: Return a `T*` to indicate a position (only)](#Rf-return-ptr)
* [F.43: Never (directly or indirectly) return a pointer or a reference to a local object](#Rf-dangle)
* [F.44: Return a `T&` when copy is undesirable and "returning no object" isn't needed](#Rf-return-ref)
* [F.45: Don't return a `T&&`](#Rf-return-ref-ref)
* [F.46: `int` is the return type for `main()`](#Rf-main)
* [F.47: Return `T&` from assignment operators](#Rf-assignment-op)
* [F.48: Don't `return std::move(local)`](#Rf-return-move-local)

其他函数规则:

* [F.50: Use a lambda when a function won't do (to capture local variables, or to write a local function)](#Rf-capture-vs-overload)
* [F.51: Where there is a choice, prefer default arguments over overloading](#Rf-default-args)
* [F.52: Prefer capturing by reference in lambdas that will be used locally, including passed to algorithms](#Rf-reference-capture)
* [F.53: Avoid capturing by reference in lambdas that will be used nonlocally, including returned, stored on the heap, or passed to another thread](#Rf-value-capture)
* [F.54: If you capture `this`, capture all variables explicitly (no default capture)](#Rf-this-capture)
* [F.55: Don't use `va_arg` arguments](#F-varargs)

函数与lambda和函数对象有很强的相似性。

**参见**: [C.lambdas: 函数对象和lambda](#SS-lambdas)

## <a name="SS-fct-def"></a>F.def: 函数的定义

函数定义是一个函数声明，以及指定函数的实现，即函数体。


### <a name="Rf-package"></a>F.1: 在函数名字中表达函数的意图

##### 原因

分解出公共代码可以使代码更具可读性，更容易重用，并限制来自复杂代码的错误。如果某个操作是一个指定良好的操作，则将其与周围的代码分开，并为其命名。

##### 示例，不要这样做！

    void read_and_print(istream& is)    // read and print an int
    {
        int x;
        if (is >> x)
            cout << "the int is " << x << '\n';
        else
            cerr << "no int on input\n";
    }

函数`read_and_print`几乎做错了所有的事情。
该函数读取`int`，然后写入`int`(到固定的`ostream`)，它也将错误信息写入(到固定的`ostream`)，但是该函数只处理`int`。该函数没有什么可重用的，逻辑上相互独立的操作混杂在一起，局部变量(`x`)超出了它们逻辑上的使用范围。对于很小的例子，看起来没有问题，但是，如果输入/输出操作以及错误处理变得更加复杂的话，混乱可能会让代码变得难以理解。

##### Note

如果要编写一个可能在多个地方使用的非平凡的(non-trivial)lambda，那么通过将其分配给一个变量(通常是非本地的)来为它命名。

##### 示例

    sort(a, b, [](T x, T y) { return x.rank() < y.rank() && x.value() < y.value(); });

命名这个lambda将表达式分解为它的逻辑部分和lambda(已命名的)部分，并为lambda的含义提供了一个强大的提示。

    auto lessT = [](T x, T y) { return x.rank() < y.rank() && x.value() < y.value(); };

    sort(a, b, lessT);
    find_if(a, b, lessT);

最短的代码并不总是能带来最好的性能和可维护性。

##### 例外

循环体，包括用作循环体的lambda很少需要被命名，然而，大的循环体(例如，有很多行或页)可能是一个问题，规则[保持函数简单而短小](#Rf-single)暗示者“保持循环体短小”。同样地，被用作回调参数的lambda有时也是非平凡的，也不可能被复用。

##### 实施

* 参见 [保持函数简单而短小](#Rf-single)
* 标记在不同地方使用的相同和非常相似的lambda。

### <a name="Rf-logical"></a>F.2: 一个函数应该只执行一个逻辑操作

##### 原因

具有单一职责的函数易于理解，测试和复用。

##### 示例

    void read_and_print()    // 糟糕
    {
        int x;
        cin >> x;
        // 错误检查
        cout << x << "\n";
    }

这是一个与特定输入绑定的整体，永远不会找到其他(不同的)用途。相反，将函数分解成合适的逻辑部分并将不同部分参数化:

    int read(istream& is)    // 更好
    {
        int x;
        is >> x;
        // 错误检查
        return x;
    }

    void print(ostream& os, int x)
    {
        os << x << "\n";
    }

这些可以在需要的地方组合在一起：

    void read_and_print()
    {
        auto x = read(cin);
        print(cout, x);
    }

If there was a need, we could further templatize `read()` and `print()` on the data type, the I/O mechanism, the response to errors, etc. Example:

如果有需要，我们可以进一步模板化`read()`和`print`的数据类型、I/O机制和错误处理等，如：

    auto read = [](auto& input, auto& value)    // better
    {
        input >> value;
        // check for errors
    };

    auto print(auto& output, const auto& value)
    {
        output << value << "\n";
    }

##### 实施

* 考虑具有多个"out"参数的可疑函数，使用返回值来替代，包括使用`tuple`来返回多个值。
* 考虑大到超过一个编辑器屏幕的可疑函数，考虑将函数分解成小巧且命名良好的子操作。
* 考虑具有7个或更多参数的可疑函数。

### <a name="Rf-single"></a>F.3: 保持函数简短

##### Reason

大函数难以阅读，很可能包含复杂代码，也很多可能包含超过最小作用域的变量。具有复杂控制结构的函数更有可能很长，也更有可能隐藏逻辑错误。

##### 示例

    double simple_func(double val, int flag1, int flag2)
        // simple_func: takes a value and calculates the expected ASIC output,
        // given the two mode flags.
    {
        double intermediate;
        if (flag1 > 0) {
            intermediate = func1(val);
            if (flag2 % 2)
                 intermediate = sqrt(intermediate);
        }
        else if (flag1 == -1) {
            intermediate = func1(-val);
            if (flag2 % 2)
                 intermediate = sqrt(-intermediate);
            flag1 = -flag1;
        }
        if (abs(flag2) > 10) {
            intermediate = func2(intermediate);
        }
        switch (flag2 / 10) {
        case 1: if (flag1 == -1) return finalize(intermediate, 1.171);
                break;
        case 2: return finalize(intermediate, 13.1);
        default: break;
        }
        return finalize(intermediate, 0.);
    }

这个太复杂了，你如何才能知道是否所有可能的情况都被正确处理了？是的，这也打破了其它规则。

我们可以重构:

    double func1_muon(double val, int flag)
    {
        // ???
    }

    double func1_tau(double val, int flag1, int flag2)
    {
        // ???
    }

    double simple_func(double val, int flag1, int flag2)
        // simple_func: takes a value and calculates the expected ASIC output,
        // given the two mode flags.
    {
        if (flag1 > 0)
            return func1_muon(val, flag2);
        if (flag1 == -1)
            // handled by func1_tau: flag1 = -flag1;
            return func1_tau(-val, flag1, flag2);
        return 0.;
    }

##### Note

"一屏无法放下"通常是“太大”的一个好的实践，1到5行的函数应当被当作是正常的。

##### Note

将大型函数分解为较小的内聚和命名的函数，简单的小函数很容易内联到函数调用成本很高的地方。

##### 实施

* 标记一屏无法显示的函数，一屏有多大？试试60行，每行140个字符，这大概是一页书所能接受的最大限度。
* 标出太复杂的函数，多复杂才算复杂？ 您可以使用环路复杂度(cyclomatic complexity)，试试"超过10条逻辑路径"，将一个简单的分支(switch)算作一个路径。

### <a name="Rf-constexpr"></a>F.4: 如果函数可能需要在编译时被求值，声明它为`constexpr`

##### 原因

 当告诉编译器允许在编译时求值时，`constexpr`是需要的。

##### 示例

(非)著名的阶乘：

    constexpr int fac(int n)
    {
        constexpr int max_exp = 17;      // constexpr让max_exp可以在Expects中使用
        Expects(0 <= n && n < max_exp);  // 防止愚蠢的输入(n<0)和溢出
        int x = 1;
        for (int i = 2; i <= n; ++i) x *= i;
        return x;
    }

这是C++14的用法，对于C++11，请使用递归形式的`fac()`。

##### Note

`constexpr`并不能保证会在编译时求值，它只是保证，如果程序员需要或编译器决定这样做可以优化时，那么可以在编译时为常量表达式参数进行求值。

    constexpr int min(int x, int y) { return x < y ? x : y; }

    void test(int v)
    {
        int m1 = min(-1, 2);            // 可能是在编译时求值
        constexpr int m2 = min(-1, 2);  // 编译时求值
        int m3 = min(-1, v);            // 运行时求值
        constexpr int m4 = min(-1, v);  // 错误：不能在运行时求值(译注：编译时无法知道v的值)
    }

##### Note

不要尝试将所有函数声明为`constexpr`，大多的计算最好是在运行时做。

##### Note

任何最终依赖高层运行时配置或业务逻辑的API都不应该被声明为`constexpr`，编译器无法对这些定义进行求值，任何有依赖这类API的`constexpr`函数都应当被重构或放弃`constexpr`。（译注：`constexpr`应当只依赖`constexpr`类型的函数）

##### 实施

(强制实施)不可能，也没有必要。当在需要常量的地方调用一个非`constexpr`函数时，编译器会给出报错。

### <a name="Rf-inline"></a>F.5: 如果函数短小且时间敏感，则将其声明为`inline`

##### 原因

有些编译器很擅长在没有程序员提示的情况下进行内联(inline)，不过不要依赖于此。测量！在过去40年的时间里，我们都被承诺说编译器可以比人类更擅长在没有提示的情况下进行内联，但是，我们还在等待这种承诺，显示地指定`inline`可以让编译器更好地工作。

##### 示例

    inline string cat(const string& s, const string& s2) { return s + s2; }

##### 例外

不要把一个`inline`函数放在一个稳定的接口中，除非你确定它不会改变，因为内联函数也是ABI的一部分（译注：如果更改了inline函数，可能会导致ABI中已经编译的部分计算结果错误）。

##### 注意

`constexpr` 暗示着 `inline`。

##### 注意

在类中定义的成员函数默认是内联的。

##### 例外

模板函数(包括模板成员函数)通常在头文件中定义，因此也是内联的。

##### 实施

将超过3行语句和可能在其它地方声明或定义(如类成员函数)的函数标记为`inline`。

### <a name="Rf-noexcept"></a>F.6: 如果函数不会抛出异常，将其声明为`noexcept`

##### 原因

If an exception is not supposed to be thrown, the program cannot be assumed to cope with the error and should be terminated as soon as possible. Declaring a function `noexcept` helps optimizers by reducing the number of alternative execution paths. It also speeds up the exit after failure.

##### Example

将`noexcept`放到所有完全使用C或其它不支持异常的语言所写的函数上，C++标准库隐式地为所有C标准库函数做了这些。

##### Note

`constexpr`函数在运行时也可能抛出异常，所以也可能也需要给这些函数加上`noexcept`。

##### Example

甚至可以在抛出异常的函数上使用`noexcept`：

    vector<string> collect(istream& is) noexcept
    {
        vector<string> res;
        for (string s; is >> s;)
            res.push_back(s);
        return res;
    }

如果`collect()`运行到内存不足，程序就是崩溃，除非程序是精心设计的能在耗尽内存后还能生存，这可以也正是我们要做的事情。`terminate()`或许可以生成合适的错误日志信息(但是，当内存不足时很难做任何有意义的事情)。

##### Note

当在决定是否给一个函数标记`noexcept`时，你必须清楚代码所运行的执行环境，特别是异常抛出和(资源/内存)分配问题。完美而通用的代码(如，标准库和其它工具代码之类)需要支持这种环境，在那里异常`bad_alloc`可能被有意义的处理。然而，大多数的程序无法有意义地处理分配失败问题，这种情况下，直接退出程序是最直观和简单的方式。如果你清楚你的应用代码无法对分配失败作出响应，或许，最好在分配函数上加上`noexcept`。

换句话说：在大多数代码中，大多数的函数可以抛出异常(例如，因为它们使用了`new`，调用了使用了`new`的函数，或者使用了以异常进行报错的库函数)，所以不要在没有考虑可能的异常是否能被处理的情况下就在所有地方放上`noexcept`。（译注：能处理异常的地方尽量去处理，不能处理的地方再使用`noexcept`）

在经常使用的代码和底层代码上，`noexcept`才是最为有用的(和最明显正确的)。

##### Note

析构函数，`swap`函数，移动操作，以及默认构造函数永远不应该抛出异常，也参见[C.44](#Rc-default00)。

##### Enforcement

<!-- 逗号应当替换成顿号  -->
* 标记不是`noexcept`，但又不会抛出异常的函数。
* 标记出会抛出异常的`swap`，`move`，析构函数和默认构造函数。

### <a name="Rf-smart"></a>F.7: 为了更通用，使用`T*`或`T&`而不是智能指针

##### 原因

应当只有为了所有权语义时才使用智能指针来转移或共享所有权（见[R.30](#Rr-smartptrparam)）。
传递智能指针限制了函数的使用，只有当调用者也使用了智能指针才可以。
传递共享的(shared)智能指针(`std::shared_ptr`)也是有运行时消耗的。

##### 示例

    // 接受任意的int*
    void f(int*);

    // 只接受你想要转移所有权的ints
    void g(unique_ptr<int>);

    // 只接受你想要共享所有权的ints
    void g(shared_ptr<int>);

    // 不改变所有权，但需要者的特定所有权
    void h(const unique_ptr<int>&);

    // 接受任意的int
    void h(int&);

##### 糟糕的示例

    // 被调用方
    void f(shared_ptr<widget>& w)
    {
        // ...
        use(*w); // 只所用了w，它的生命周期压根没有被使用到
        // ...
    };

更深入请参考 [R.30](#Rr-smartptrparam).

##### Note

我们可以静态地捕获到悬垂指针，所以不需要依赖于资源管理来避免悬垂指针。

**也参见**:

* [当"没有参数"是一个合法选项是，更倾向于`T*`而不是`T&`](#Rf-ptr-ref)
* [智能指针规则总结](#Rr-summary-smartptrs)

##### 实施

标出参数是智能指针类型(重载了`operator->`或`operator*`的类型)，但是所有权语义没有被用到的(函数)，那是：

* 可复制但是从未被复制/移动，或可移动但是从未被移动。
* 从未被修改，或传递到其它也可能这样做的函数。

### <a name="Rf-pure"></a>F.8: 优先使用纯函数(pure function)

##### 原因

纯函数更容易理解，更容易优化(甚至并行化)，以及有时可以备注(memoized)。

##### 示例

    template<class T>
    auto square(T t) { return t * t; }

##### 实施

不可能强制实施

### <a name="Rf-unused"></a>F.9: 未使用的参数应该是匿名的

##### 原因

可读性，抑制警告未使用的参数。 (译注：编译器会对未使用的参数发出警告，匿名参数可以解决这个问题)

##### 示例

    X* find(map<Blob>& m, const string& s, Hint);   // 曾几何时使用过Hint，但是现在不再使用了

##### Note

20世纪80年代早期引入了匿名参数来解决这个问题。

##### 实施

标出未使用的命名参数。

## <a name="SS-call"></a>F.call: 参数传递

有很多方法来向函数传参和返回值。

### <a name="Rf-conventional"></a>F.15: 使用简单而传统的方法来传递信息

##### 原因

使用“不寻常的和聪明的”技巧会导致意外，增加其他程序员的理解难度，更可能引入bug。
如果你真的觉得需要一个常见的技术之外优化技术，测量以确保它确实是一个改进，写好文档和注释，因为这个改进可能是不可移植的。

下面的表格总结了F.16-21的建议。

普通的参数传递：

![普通的参数传递](./param-passing-normal.png "Normal parameter passing")

高级的参数传递:

![高级的参数传递](./param-passing-advanced.png "Advanced parameter passing")

只有在确认需求之后才使用高级技巧，并在注释中写明需求。

### <a name="Rf-in"></a>F.16: 对于输入参数，对于复制廉价的类型使用传值的方式，其余的使用`const`引用类型

##### 原因

两种方式都是为了让调用者知道函数不会修改它的参数，并允许使用右值进行初始化。

"复制廉价"的含义取决于机器架构，两个或三个word(double, 指针，引用)最好使用值的方式进行传递。当复制是廉价的，不会打破简洁性和复制的安全性，对于两三个word的小对像，传值会比引用更快，那是因为少了引用在函数中访问时的间接性。

##### Example

    void f1(const string& s);  // OK: 使用常量引用，很廉价

    void f2(string s);         // bad: 可能会很费

    void f3(int x);            // OK: 无敌的

    void f4(const int& x);     // bad: f4()中访问是有开销(overhead)

(只)在高级用法中需要对“仅仅输入”的参数进行右值优化时：

* 如果函数会无条件地移动(move)它的参数，使用`&&`，参见[F.18](#Rf-consume)。
* 对于右值，如果函数想要复制这个参数，除了以`const&`进行传递外，以`&&`类型对函数进行重载，并在函数体中使用`std::move`复制到目标，该函数本质上重载了一个`将要移动出`的语义，参见[F.18](#Rf-consume)。
* 特殊情况下，例如多个"输入+输出"的参数，考虑使用`完美转发`(perfect forwarding)，参见[F.19](#Rf-forward)。

##### Example

    int multiply(int, int); // 只输入了ints，传值

    // 尾参是以const&传递的输入参数，没有int那么廉价
    string& concatenate(string&, const string& suffix);

    void sink(unique_ptr<widget>);  // 只做输入用，移动了widget的所有权

避免“深奥的技术”，例如：

* “为了省事”使用`T&&`进行传参，关于`&&`性能优势的大部分传言都是假的或不可靠的，参见[F.18](#Rf-consume)和[F.19](#Rf-forward)。
* 从赋值中返回`const T&`以及相似的操作，参见[F.47](#Rf-assignment-op)。

##### 示例

假定`Matrix`具有移动操作(可能在`std::vector`中保存了元素)：

    Matrix operator+(const Matrix& a, const Matrix& b)
    {
        Matrix res;
        // ... fill res with the sum ...
        return res;
    }

    Matrix x = m1 + m2;  // 移动构造

    y = m3 + m3;         // 移动赋值

##### Notes

返回值优化不会处理赋值的情况，但是移动赋值会。

可以假定引用指向了一个合法的对象(语言规则)，没有(合法的)“空引用”。如果你需要一个可选的概念，使用指针、`std::optional`、或能表示“没有值”的特殊值。

##### Enforcement

* (简单) ((基础)) 如果一个大小大于`2 * sizeof(void*)`的参数按值传递，则发出警告，推荐使用`const`的引用来代替。
* (简单) ((基础)) 如果一个大小小于`2 * sizeof(void*)`的参数按引用传递，则发出警告，推荐使用按值传递。
* (简单) ((基础)) 警告一个以`const`引用进行传递的被`move`参数。

### <a name="Rf-inout"></a>F.17: 对于"输入/输出"参数，以非`const`引用进行传递

##### Reason

可以让调用者清楚地知道这个对象是可能被修改的。

##### Example

    void update(Record& r);  // 假定了update会写入r

##### Note

`T&`类型的参数可以向函数中传入信息，也可以传出信息。因此`T&`可以是输入输出参数，这本身可能就是一个问题和错误的来源。

    void f(string& s)
    {
        s = "New York";  // 没有明显的错误
    }

    void g()
    {
        string buffer = ".................................";
        f(buffer);
        // ...
    }

这里，`g()`的作者给`f()`提供了一块buffer进行填充，但是，`f()`简单地替换了它（用了一种比简单复制字符更高点的代价）。
如果`g()`的作者错误地假定了`buffer`的大小，则可能会导致严重的逻辑错误。


##### Enforcement

* (Moderate) ((基础)) 如果函数有非`const`引用的参数，但是并没有更改该参数，则发出警告。
* (简单) ((基础)) 如非`const`引用的参数被`std::move`，则发出警告。

### <a name="Rf-consume"></a>F.18: 对于“将被移动”的参数，使用`X&&`传递和`std::move`移动之

##### Reason

这是有效的，且在调用点上消除了bug：`X&&`绑定到右值(rvalue)，如果要传递左值，需要在调用位置显示地使用`std::move`。

##### Example

    void sink(vector<int>&& v) {   // sink 拥有所有参数的所有权
        // 通常，这里对会v进行const访问(译注：即不会更改v)
        store_somewhere(std::move(v));
        // 通常，这里不会再对v进行使用；它已经被move了
    }

Note that the `std::move(v)` makes it possible for `store_somewhere()` to leave `v` in a moved-from state.
[That could be dangerous](#Rc-move-semantic).

注意，`std::move(v)`让`store_somewhere()`把`v`变成已移动(moved-from)状态变成可能，不过[这可能是危险的](#Rc-move-semantic)。

##### 例外

唯一拥有者类型(Unique owner type)是只可以移动的，不过很廉价，例如`unique_ptr`，也可以进行按值传递，这样即简单又能达到同样的目的。按值传递会产生一个额外但廉价的move操作，但更简单明了。

例如：

    template <class T>
    void sink(std::unique_ptr<T> p) {
        // 使用 p ... 可能在有些地方用到std::move(p)
    }   // p 被销毁了

##### 实施

* 标记所有参数是`X&&`(其中`X`不是模板类型参数名)，但函数体内并没有对其进行`std::move`操作的函数。
* 标记对已移动(moved-from)状态对象的访问。
* 不要有条件地移动对象。

### <a name="Rf-forward"></a>F.19: 对于要"转发"的参数，使用`TP&&`并只`std::forward`它

##### 原因

如果要将对象传递给其他代码，而该函数又不直接使用该对象，那么我们希望该函数不受`const`和右值属性的影响。

当且仅当在那种情况下，让参数为`TP&&`, 其中`TP`是模板类型参数 -- 它即*忽略*又*保留*该参数的`const`属性和左传属性。因此，任何使用`TP&&`的代码都预示着其不关心变量的`const`属性和右值属性（被忽略了），但它将该变量传递给的代码却要关心它的`const`属性和右值属性（因为它保留了）。使用`TP&&`的参数是安全的，因为任何从调用者传进来的临时对象都会在整个函数调用期间有效。`TP&&`类型的参数本质上应该通过函数体中的`std::forward`继续（向其它函数）传递。

##### 示例

    template <class F, class... Args>
    inline auto invoke(F f, Args&&... args) {
        return f(forward<Args>(args)...);
    }

    ??? 调用 ???

##### 实施

* 标记一个函数，该函数接受一个`TP&&`参数(`TP`是一个模板类型参数名)，并在每个静态路径上对它执行除`std::forward`之外的任何操作。

### <a name="Rf-out"></a>F.20: 对于输出值，优先使用返回值，而不是输出参数

##### 原因

返回值是更直白，而`&`即可以是输入/输出，也可以是只输出的，很容易被误用。

大对象像标准库中的容器，使用隐式的move操作来提高性能和避免显示的内存管理。

如果你有多个值要返回，使用[tuple](#Rf-out-multi)或类似的多成员类型。

##### 示例

    // OK: 返回值为x的元素的指针
    vector<const int*> find_all(const vector<int>&, int x);

    // Bad: 将值为x的元素的指针放到in-out参数中
    void find_all(const vector<int>&, vector<const int*>& out, int x);

##### Note

移动(move)一个有很多元素构成的`struct`的代价可能是昂贵的（每个单独的元素是move廉价的）。

不建议返回一个`const`值。这个旧的建议(返回const值)现在过时了，它不会带来收获，并且会干扰移动语义。

    const vector<int> fct();    // bad: “const”比它的价值更麻烦

    vector<int> g(const vector<int>& vx)
    {
        // ...
        fct() = vx;   // 被"const"阻止了
        // ...
        return fct(); // 昂贵的复制：移动语义被"const"抑制了
    }

添加`const`返回值的理由是它可以阻止(很少)意外地访问临时变量。
反对的理由是它会阻止(非常频繁)移动语义的使用。

##### 例外

* 对于非值类型，例如在继承体系下的类型，使用`unique_ptr`或`shared_ptr`返回对象。
* 如果一个类型的移动代价很高(如，`array<BigPOD>`)，考虑将其分配在自由存储区(译注：堆内存空间)，然后返回一个句柄(如，`unique_ptr`)，或将目标对象的非`const`引用传递到函数进行填充(用作输出参数)。
* 为了在函数内循环的多次调用中，重用一个具有容量的对象(如，`std::string`、`std::vector`)：[将其作为in/out参数并通过引用传递](#Rf-out-multi).

##### 示例

    struct Package {      // 例外情况：移动昂贵的对象
        char header[16];
        char load[2024 - 16];
    };

    Package fill();       // Bad: 大的返回值
    void fill(Package&);  // OK

    int val();            // OK
    void val(int&);       // Bad: 函数val会读它的参数吗？

##### Enforcement

* 标出一个函数，如果非`const`引用的参数在写之前并没有被读过，并且该类型可以被廉价地返回；应当使用返回值进行输出。
* 标出返回`const`值的函数。修改：去掉`const`，使用非`const`返回值进行替代。

### <a name="Rf-out-multi"></a>F.21: 为了返回多个输出值，优先使用strcut或tuple进行返回

##### 原因

返回值作为"仅输出"是自明(自我描述)的。
注意，C++确实有多返回值，习惯上使用`tuple`(包括`pair`)，在调用位置上使用`tie`可能会带来额外的便利。
倾向于使用一个具有返回值语义的命名结构体，另外，不具名的`tuple`在泛型代码有很有用。

##### Example

    // BAD: ”仅输出“参数在注释中注明
    int f(const string& input, /*仅输出*/ string& output_data)
    {
        // ...
        output_data = something();
        return status;
    }

    // GOOD: 自解释的
    tuple<int, string> f(const string& input)
    {
        // ...
        return make_tuple(status, something());
    }

C++98的标准库已经使用了这种(多返回值)风格，因为`pair`就像是两个元素的`tuple`。
例如，给出一个`set<string> my_set`，考虑：

    // C++98
    result = my_set.insert("Hello");
    if (result.second) do_something_with(result.first);    // 工作区

使用C++11，我们可以这样写，将结果直接写入已经存在的局部变量：

    Sometype iter;                                // 默认初始化，如果还没准备好
    Someothertype success;                        // 为一些其它目的而使用这些变量

    tie(iter, success) = my_set.insert("Hello");   // 正常返回
    if (success) do_something_with(iter);

使用C++17，我们可以使用"结构化绑定"来声明我初始化多个变量：

    if (auto [ iter, success ] = my_set.insert("Hello"); success) do_something_with(iter);

##### 例外

有时，为了操作一个对象我们需要将其传递到函数内，在这种情况，使用引用[`T&`](#Rf-inout) 来传递对象通常是正确的。显示地将传进去的in-out参数再使用返回值输出出来一般是没有必要的，例如：

    istream& operator>>(istream& is, string& s);    // 很像 std::operator>>()

    for (string s; cin >> s; ) {
        // 对每行进行处理
    }

这里，`s`和`cin`都被用作in-out参数。我们使用非`const`引用来传递`cin`，以便能操作它的状态。传递`s`以避免重复分配，通过传递引用来复用`s`，只有需要扩大`s`的容量时才分配新的内存，这项技术有时也被称为"调用者分配输出"(caller-allocated out)模式，对`string`和`vector`这些需要自由存储空间的类型特别有用。

作为对比，如果我们将所有的值以返回值的方式进行返回，可以就像这样：

    pair<istream&, string> get_string(istream& is);  // 不推荐
    {
        string s;
        is >> s;
        return {is, s};
    }

    for (auto p = get_string(cin); p.first; ) {
        // do something with p.second
    }

我们认为这是明显不优雅和缺乏效率的。

如果真正认真的阅读这条规则(F.21)，这个例外情况并非是真正的例外，因为它依赖了in-out参数，而不是规则中提到的普通out参数，然而，我们更喜欢明确的规则，而不是微妙的(规则)。

##### Note

在很多例子中，返回特定的、用户定义的类型可能是有用的，例如：

    struct Distance {
        int value;
        int unit = 1;   // 1 代表米
    };

    Distance d1 = measure(obj1);        // 访问 d1.value 和 d1.unit
    auto d2 = measure(obj2);            // 访问 d2.value 和 d2.unit
    auto [value, unit] = measure(obj3); // 访问 value 和 unit; 对于知道measure()的人来说，多少有点冗余
    auto [x, y] = measure(obj4);        // 不要这样做：可能会让人迷惑

过于泛化的`pair`和`tuple`应当只用在返回值表示独立实体而非一个抽象的情况下。

另一个例子，使用类似于`variant<T, error_code>`的特化类型，而不泛型的`tuple`。

##### Enforcement

* 输出参数应当被替换成返回值。
其中，输出参数是指调用非`const`成员函数写入的成员变量，或以非`const`引用传递的参数。

### <a name="Rf-ptr"></a>F.22: 使用`T*`或`owner<T*>`来指定单个对象

##### 原因

可读性：让普通指针的函数的含义更加清晰，能增加有意义的工具支持。

##### Note

在传统的C和c++代码中，普通的`T*`用于许多不怎么相关的(弱相关的)目的，比如:

* 标识(单个)对象(不会删除被此函数删除)。
* 指向一个在自由存储区上分配的对象(稍后删除)。
* 持有`nullptr`。
* 标识C风格的字符串(以`\0`结尾的字符数组)。
* 标识一个长度分开指定的数组。
* 标识数组中的位置。

这让代码在做什么以及应当做什么变得难以理解，也让检查和工具支持变得复杂。

##### 示例

    void use(int* p, int n, char* s, int* q)
    {
        p[n - 1] = 666; // Bad: 我们不知道p是否指向n个元素，如果是，使用span<int>
        cout << s;      // Bad: 我们不知道s是否指向\0结尾的char数组，如果是，使用zstring
        delete q;       // Bad: 我们不知道*q是否分配在自由存储区，如果是，使用owner
    }

更好

    void use2(span<int> p, zstring s, owner<int*> q)
    {
        p[p.size() - 1] = 666; // OK, 范围错误可以被捕获到
        cout << s; // OK
        delete q;  // OK
    }

##### Note

`owner<T*>`代表所有权，`zstring`代表C风格的字符串。

**同样**: 假定从指向`T`的智能指针(如，`unique_ptr<T>`)中获取的`T*`指向单个元素。

**也参见**: [支持库](#S-gsl)

**也参见**: [不要以单个指针的形式传递数组](#Ri-array)

##### Enforcement

* (简单) ((有限)) 对于指针类型表达式上的任何算术(arithmetic)运算，如果运算结果为指针类型值，则发出警告。

### <a name="Rf-nullptr"></a>F.23: 使用`not_null<T>`来表示"null"不是一个有效值

##### 原因

直观：有`not_null<T>`参数的函数表明，它的调用函数有责任在必要的地方进行所有的`nullptr`检查；类似地，返回值为`not_null<T>`的函数表明，调用它的函数不需要检查`nullptr`。

##### 示例

`not_null<T*>`能够明显地读者(人或机器)知道在解引用(dereference)之前检查`nullptr`是没有必要的。
另外，在debug时，`owner<T*>`和`not_null<T>`可以被工具检查是否正确。

考虑:

    int length(Record* p);

当我调用`length(p)`时，我应该检查`p`是否为`nullptr`吗？`length()`的实现应该检查`p`是否为`nullptr`吗？

    // 保证p != nullptr是调用者的事
    int length(not_null<Record*> p);

    // length()的实现者必须假定p == nullptr是有可能的
    int length(Record* p);

##### Note

`not_null<T*>`是假定不会为`nullptr`的，而`T*`可能为`nullptr`，两者都可以在内存中表示`T*`(所以没有隐含的运行时开销)。

##### Note

`not_null`不仅适用于内置的指针类型，也适用于`unique_ptr`、`shared_ptr`以及其它像指针的类型。

##### Enforcement

* (简单) 如果一个函数对原始指针进行解引用之前没有检查`nullptr`(或等价的)，则给出警告，建议声明为`not_null`。
* (简单) 如果一个函数对原始指针进行解引用之前有时先进行了`nullptr`(或等价的)检查，有时又没有进行检查，则报错。
* (简单) 如果一个函数对`not_null`进行了`nullptr`检查，则给出警告。

### <a name="Rf-range"></a>F.24: 使用`span<T>`或`span_p<T>`来指定一个半开序列(half-open sequence)

##### Reason

非正式/非显式范围是错误的来源。

##### Example

    X* find(span<X> r, const X& v);    // 在r中查找v

    vector<X> vec;
    // ...
    auto p = find({vec.begin(), vec.end()}, X{});  // 在vec中查找X{}

##### Note

范围(Range)在C++代码中相当普通，通常它们都是隐式，并且很难保证能正确使用它们。
特别是当用一对参数`(p, n)`来指定一个数组`[p:p+n)`时，通常不太可能知道`*p`后面是否确实跟了`n`个元素。
`span<T>`和`span_p<T>`是简单的辅助类，分别用来指定范围`[p:q)`和一个以`p`开头及让断言(predicate)为真的第一个元素为结尾的范围。

##### Example

`span`表示了元素的范围，但是我们怎么样操作范围内的元素呢？

    void f(span<int> s)
    {
        // 范围遍历（保证正确）
        for (int x : s) cout << x << '\n';

        // C风格的遍历（潜在的检查）
        for (gsl::index i = 0; i < s.size(); ++i) cout << s[i] << '\n';

        // 随机访问（潜在的检查）
        s[7] = 9;

        // 提取指针（潜在的检查）
        std::sort(&s[0], &s[s.size() / 2]);
    }

*译注：C风格的遍历、随机访问、提取指针等操作时，span会进行范围的合法性检查，这是有一定的开销的(overhead)。*

##### Note

`span<T>`对象没有持有元素，所以小到可以按值传递。

传递`span`对象作为参数跟传递一对指针或指针和一个整型的数量一样高效。

**也参见**: [支持库](#S-gsl)

##### Enforcement

(复杂) 发出警告，如果访问的指针被其它整数类型参数进行范围限定时，建议他们使用`span`替代。

### <a name="Rf-zstring"></a>F.25: 使用`zstring`或`not_null<zstring>`来指定C风格的字符串

##### Reason

C风格的字符串无处不在，它们由约定定义：以`\0`结尾的字符数组。
我们必须将C风格的字符串与指向单个字符的指针或指向字符数组的老式指针区分开来。

##### Example

考虑:

    int length(const char* p);

当调用`length(s)`时，我应该先检查`s`是否为`nullptr`吗？`length()`的实现应该检查`p`是否为`nullptr`吗？

    // length()的实现必须假定p == nullptr是有可能的
    int length(zstring p);

    // 调用者有责任保证p != nullptr
    int length(not_null<zstring> p);

##### 注意

`zstring`不代表所有权。

**也参见**: [支持库](#S-gsl)

### <a name="Rf-unique_ptr"></a>F.26: 在需要指针的地方使用`unique_ptr<T>`来转移所有权

##### Reason

使用`unique_ptr`是安全地传递指针最廉价的方式。

**也参见**: [C.50 关于何时从工厂返回`shared_ptr`](#Rc-factory)

##### 示例

    unique_ptr<Shape> get_shape(istream& is)  //  从输入流组装形状 
    {
        auto kind = read_header(is); // 在输入端读取头部，及识别下一个形状
        switch (kind) {
        case kCircle:
            return make_unique<Circle>(is);
        case kTriangle:
            return make_unique<Triangle>(is);
        // ...
        }
    }

##### Note

当使用接口使用类层次结构中的对象时，你需要传递一个(基类)指针，而不是对象。

##### Enforcement

(简单) 当函数返回一个在局部分配的原始指针时，给出警告，建议使用`unique_ptr`或`shared_ptr`进行替换。

### <a name="Rf-shared_ptr"></a>F.27: 使用`shared_ptr<T>`来共享所有权

##### 原因

表达共享所有权的标准做法是使用`std::shared_ptr`，也就是说，由最后一个拥有者来删除对象。

##### 示例

    shared_ptr<const Image> im { read_image(somewhere) };

    std::thread t0 {shade, args0, top_left, im};
    std::thread t1 {shade, args1, top_right, im};
    std::thread t2 {shade, args2, bottom_left, im};
    std::thread t3 {shade, args3, bottom_right, im};

    // 分离(detach)线程
    // 最后一个线程结束时删除image

##### Note

如果同一时刻只有一个所有者，优先使用`unique_ptr`，而不是`sharred_ptr`。
`shared_ptr`是为了共享所有权的(译注：即同一时刻会有多个所有者)。

注意，到处所有`shared_ptr`是有开销的(`shared_ptr`的引用计数的原子操作会有一个可观的累积消耗)。

##### 可选方案

让一个对象拥有共享对象(例如，作用域对象)，并在所有用户完成时销毁它(最好是隐式地)。

##### Enforcement

(不可强制) 这是一个过于复杂的模式，以至于无法可靠地检测。

### <a name="Rf-ptr-ref"></a>F.60: 当"没有参数"("no argument")是合法的选项时，使用`T*`而不是`T&`

##### 原因

指针(`T*`)可以是`nullptr`，而引用(`T&`)却没有合法的空引用(null reference)。
有时使用`nullptr`作为指示“没有对象”的替代选项是有用的，但是如果不是，引用就简单得多，并且可能产生更好的代码。

##### 示例

    string zstring_to_string(zstring p) // zstring是char*，一个C风格的字符串
    {
        if (!p) return string{};    // p可能是nullptr，记得需要检查
        return string{p};
    }

    void print(const vector<int>& r)
    {
        // r指向一个vector<int>，不需要检查
    }

##### Note

构造一个实质上是`nullptr`的引用是可能的，但不是有效的C++，例如`T* p = nullptr; T& r = (T&)*p;`，这种错误很少见。

##### Note

如果你喜欢指针表示法(`->`或`*`而不是`.`)，`not_null<T*>`提供了像`T&`一样的保证。

##### Enforcement

* Flag ???

### <a name="Rf-return-ptr"></a>F.42: (仅)在表示位置时返回`T*`

##### 原因

这是指针的用处，返回`T*`来转移所有权是一种误用。

##### 示例

    Node* find(Node* t, const string& s)  // 在Node的二叉树中查找s
    {
        if (!t || t->name == s) return t;
        if ((auto p = find(t->left, s))) return p;
        if ((auto p = find(t->right, s))) return p;
        return nullptr;
    }

如果`find`返回的指针不为`nullptr`，说明有一个`Node`持有`s`。
重要的是，这并不意味着将指向对象的所有权转移给调用者。

##### Note

位置也可以通过迭代器、索引和引用来转移。
相比于指针，引用通常是更好的选择，[如果无需使用`nullptr`](#Rf-ptr-ref) 或 [被引用对象不应改变](???)

##### Note

不要返回不在调用者作用域内的对象的指针；参见[F.43](#Rf-dangle)。

**也参见**: [discussion of dangling pointer prevention](#???)

##### Enforcement

<!-- * Flag `delete`, `std::free()`, etc. applied to a plain `T*`.
Only owners should be deleted.
* Flag `new`, `malloc()`, etc. assigned to a plain `T*`.
Only owners should be responsible for deletion. -->

* 标出作用于普通`T*`上的`delete`、`std::free()`等，只有拥有者应当被删除。
* 标出分配给普通`T*`的`new`, `malloc()`等，只能拥有者有责任进行删除。

### <a name="Rf-dangle"></a>F.43: 永远不要(直接或间接地)返回指向局部对象的指针或引用

##### 原因

为了避免crash和可能会导致使用悬垂指针的数据损坏。

##### 糟糕的示例

当函数返回后，局部对象都不再存在：

    int* f()
    {
        int fx = 9;
        return &fx;  // 糟糕
    }

    void g(int* p)   // 看起来很无辜
    {
        int gx;
        cout << "*p == " << *p << '\n';
        *p = 999;
        cout << "gx == " << gx << '\n';
    }

    void h()
    {
        int* p = f();
        int z = *p;  // read from abandoned stack frame (bad) 从已经丢弃的栈帧上读取 (糟糕)
        g(p);        // pass pointer to abandoned stack frame to function (bad) 传递指向已经丢弃的栈帧的指针到函数 (糟糕)
    }

在一个流行的实现上，我得到了输出:

    *p == 999
    gx == 999

这是我所期望的，因为`g()`的调用复用了被丢弃了的`f()`调用时的栈帧，所以`*p`指向的空间现在被`gx`占用。

* 想像一下，如果`fx`和`gx`是不同的类型会发生什么。
* 想像一下，如果`fx`和`gx`是不变量类型会发生什么。
* 想像一下，如果在很多的函数之间传递更多的悬垂指针会发生什么。
* 想像一下，黑客可能会用悬垂指针做什么。

幸运的是，大多数(全部?)的现代编译器都能捕获并警告这种简单的情况。


##### Note

这也适用于引用：

    int& f()
    {
        int x = 7;
        // ...
        return x;  // Bad: 返回即将销毁对象的引用
    }

##### Note

这只适用于非`static`的局部变量，所有的`static`变量(如它们的名字所示)都是静态分配的，所以指向他们的指针不会悬垂(dangle)。

##### 糟糕的示例

并非所有指向局部变量的指针泄漏都是那样明显的：

    int* glob;       // 全局变量在很多方面都很糟糕

    template<class T>
    void steal(T x)
    {
        glob = x();  // BAD
    }

    void f()
    {
        int i = 99;
        steal([&] { return &i; });
    }

    int main()
    {
        f();
        cout << *glob << '\n';
    }

在这里，我设法读到了被`f`的调用所丢弃的位置。
存储在`glob`中的指针可以在更晚的时候被使用，并以不可预知的方式造成麻烦。

##### Note

局部变量地址泄漏的方式有：return语句、`T&`输出参数、返回对象的成员变量、返回数组的元素、以及更多。

##### Note

可以构造类似内部作用域"泄漏"到外部作用域这样的例子，对这类例子的处理类似于指针泄漏到函数外。

这个问题的一个稍微不同的变种是，将指针放到超过对象生命期的容器中。

**也见** 获取悬垂指针的另一种方式是[指针失效](#???)，可以用类似的技术来检测和预防。。

##### Enforcement

* 编译器倾向于捕获对局部变量的引用的返回，并且在很多情况下可以捕获指向局部变量的指针的返回。
* 静态分析可以捕获许多使用指针表示位置的常见模式(从而消除悬空指针)。

### <a name="Rf-return-ref"></a>F.44: 当不需要复制且不需要返回空对象时，返回`T&`

##### 原因

C++语言保证了`T&`引用一个对象，所以不需要检查`nullptr`。

<!-- [discussion of dangling pointer prevention](#???) and [discussion of ownership](#???). -->
**也参见** 返回引用必须没有隐含的所有权转移：[预防悬垂指针的讨论](#???)和[所有权的讨论](#???)

##### 示例

    class Car
    {
        array<wheel, 4> w;
        // ...
    public:
        wheel& get_wheel(int i) { Expects(i < w.size()); return w[i]; }
        // ...
    };

    void use()
    {
        Car c;
        wheel& w0 = c.get_wheel(0); // w0的生命周期与c相同
    }

##### 实施

标记出没有`return`语句可以返回`nullptr`的函数。

### <a name="Rf-return-ref-ref"></a>F.45: 不要返回`T&&`

##### 原因

它请求返回对已销毁临时对象的引用，`&&`和临时对象是联系在一起的。

##### 示例

返回的右值引用在整个返回表达式结束之后也就超出的作用域范围。

    auto&& x = max(0, 1);   // 目前不是没有问题的
    foo(x);                 // 未定义的行为(译注：已经超出了右值引用的作用域范围)

这种使用(返回右值引用)是常见的错误来源，经常被错误地报告为编译器bug。
函数的实现者应该避免为用户设置这样的陷阱。

[lifetime safety profile](#SS-lifetime)(在完全完成后)捕获这种问题。

##### 示例

Returning an rvalue reference is fine when the reference to the temporary is being passed "downward" to a callee;
then, the temporary is guaranteed to outlive the function call (see [F.18](#Rf-consume) and [F.19](#Rf-forward)).
However, it's not fine when passing such a reference "upward" to a larger caller scope.
For passthrough functions that pass in parameters (by ordinary reference or by perfect forwarding) and want to return values, use simple `auto` return type deduction (not `auto&&`).

假设`F`是返回“值类型”的

    template<class F>
    auto&& wrapper(F f)
    {
        log_call(typeid(f)); // 或其它仪表化工具
        return f();          // 糟糕：返回了临时对象的引用
    }

更好:

    template<class F>
    auto wrapper(F f)
    {
        log_call(typeid(f)); // 或其它仪表化工具
        return f();          // OK
    }


##### 例外

`std::move`和`std::forward`确实返回了`&&`，但是它们只是强制类型转换 -- 约定仅在表达式上下文中使用，其中对临时对象的引用在销毁临时对象之前在同一表达式中传递。关于返回`&&`，我们不知道还有什么更好的例子。

##### 实施

标记除`std::move`和`std::forward`之外任何返回`&&`的函数。

### <a name="Rf-main"></a>F.46: `int`是`main()`的返回值类型 

##### 原因

这是一个语言规则，但是由于“语言扩展”的存在而经常被违反，因此值得一提：声明`main`(程序入口)为`void`限制了可移植性。

##### 示例

        void main() { /* ... */ };  // 糟糕，不是C++

        int main()
        {
            std::cout << "This is the way to do it\n";
        }

##### Note

我们之所以提到这一点，是因为这个错误在社区中持续存在。

##### 实施

* 应该编译器为做。
* 如果编译器没做，用工具标出它。

### <a name="Rf-assignment-op"></a>F.47: 从赋值操作符返回`T&`

##### 原因

运算符重载(特别是值类型)的习惯是由`operator=(const T&)`执行赋值，然后返回(非`const`的)`*this`。这保证了与标准库类型的一致性，以及遵循了"像int那样做"(do as the ints do)。

##### Note

从历史上看，有一些指导要求赋值操作符返回`const T&`，这主要是为了避免`(a = b) = c`的形式 -- 这些代码并没有常见到可以说明违反了与标准类型的一致性。

##### 示例

    class Foo
    {
     public:
        ...
        Foo& operator=(const Foo& rhs) {
          // 复制成员变量.
          ...
          return *this;
        }
    };

##### 实施

应当使用工具来强制性地检查赋值操作符的返回类型(以及返回值)。

### <a name="Rf-return-move-local"></a>F.48: 不要`return std::move(local)`

##### 原因

有了保证的复制消除(copy elision)，现在在返回语句中明确使用`std::move`几乎是一种多余的做法。

##### 糟糕的例子

    S f()
    {
      S result;
      return std::move(result);
    }

##### 好的例子

    S f()
    {
      S result;
      return result;
    }

##### 实施

应当使用工具来强制性地检查返回表达式。

### <a name="Rf-capture-vs-overload"></a>F.50: 当函数不起作用时使用lambda(捕获局部变量，或编写局部函数) Use a lambda when a function won't do (to capture local variables, or to write a local function)

##### 原因

函数不能捕获局部变量或在局部范围内声明；如果你需要这些东西，在可能的情况下选择lambda，在不需要的地方选择手写的函数对象。另一方面，lambda和函数对象不能进行重载；如果需要重载，最好使用函数(这些地方会让lambda过于浮夸)。如果这两种方法都可行，最好编写一个函数(译注：函数可以复用，Lambda却不可以复用)；使用最简单的工具。

##### 示例

    // 写一个只接受int或string的函数，重载是自然的选择
    void f(int);
    void f(const string&);

    // 写一个需要声明或表达式作用域内捕获局部状态的函数，lambda是自然的选择
    vector<work> v = lots_of_work();
    for (int tasknum = 0; tasknum < max; ++tasknum) {
        pool.run([=, &v]{
            /*
            ...
            ... process 1 / max - th of v, the tasknum - th chunk
            ...
            */
        });
    }
    pool.join();

##### 例外

泛型lambda为编译函数模板提供了一个简单的方式，即使是普通函数模板只需要多一点点的语法也能达到同样目标的情况下也是有用的。不过，未来一旦所有的函数都能拥有概念参数(Concept parameter)，这种优势就会消失。

##### Enforcement

* 如果一个具名的泛型lambda(例如，`auto x = [](int i){ /*...*/; };`)的capture列表为空且定义在全局空间，则给出警告，使用普通函数代替。

### <a name="Rf-default-args"></a>F.51: 如果可以，选择默认参数而不是重载

##### 原因

默认参数只为单个实现提供了可选的接口；不能保证一组重载函数都实现相同的语义；使用默认参数可以避免代码重复。

##### Note

当可选项来自一组相同类型的参数时，可以在使用默认参数和重载之间进行选择，例如:

    void print(const string& s, format f = {});

截然相反的是：

    void print(const string& s);  // 使用默认参数
    void print(const string& s, format f);

当一组函数用于对一组类型执行语义上等价的操作时，则没有选择，例如:

    void print(const char&);
    void print(int);
    void print(zstring);

##### 参见

[虚函数的默认参数](#Rh-virtual-default-arg)

##### 实施

* 给出警告，如果一组重载函数具有相同的前缀参数(例如，`f(int)`、 `f(int, const string&)`、 `f(int, const string&, double)`)。注意，如果在实践中不太实用，请检查该实施措施。

### <a name="Rf-reference-capture"></a>F.52: 如果只是局部地使用，包括传递给算法，选择在lambda中使用引用进行捕获

##### Reason

为了提高效率和正确性，在局部使用lambda时，您几乎总是希望通过引用来捕获变量，这包括在编写或调用本地的并行算法，因为它们在返回之前是连接在一起的。

##### 讨论

考虑到效率，大多数类型通过引用传递比通过值传递消耗更少。

考虑到正确性，很多调用都想在调用点上对原始对象作出修改(参见下面的示例)，按值传递无法做到。

##### Note

不幸的是，没有一种简单的方式通过捕获`const`引用来既获得效率又阻止副作用(译注：副作用即是对变量做出变量)。

##### 示例

这里，一个大对象(网络消息)传递给了迭代算法，对消息的复制即不高效也不正确(可能不可复制)。

    std::for_each(begin(sockets), end(sockets), [&message](auto& socket)
    {
        socket.send(message);
    });

##### 示例

这是一个简单的三阶段(stage)并行流水线，每个`stage`对象都封装了一个工作线程和一个队列，有一个`process`函数作用于进入队列的任务，并且它的析构函数会在线程结束前自动地阻塞等待队列清空。

    void send_packets(buffers& bufs)
    {
        stage encryptor([] (buffer& b){ encrypt(b); });
        stage compressor([&](buffer& b){ compress(b); encryptor.process(b); });
        stage decorator([&](buffer& b){ decorate(b); compressor.process(b); });
        for (auto& b : bufs) { decorator.process(b); }
    }  // 自动地阻塞，等待流水线结束

##### Enforcement

标出通过引用捕获lambda，但不是在函数作用域中本地使用，也不是通过引用传递给函数。(注意：这条规则是一个近似值，但是要标出通过指针传递的lambda，这些指针更有可能由被调用者存储、通过访问参数写入堆内存、返回lambda，等等。`生命周期规则`还将提供标记转义指针和引用(包括通过lambda)的通用规则。)


### <a name="Rf-value-capture"></a>F.53: 避免lambda捕获引用，如果不是本地使用，包括返回、堆上存储、传递给其它线程

##### 原因

局部变量的指针和引用不能超过他们的作用域。lambda捕获的引用只是存储本地变量引用的另一个地方，如果超过了它们(或引用的副本)的作用域，就不应该这样做。

##### 糟糕的例子

    int local = 42;

    // 想要一个local的引用，注意，当程序离开这个作用域后，
    // 局部变量不再存在，因此，process()的调用会有未定义的行为
    thread_pool.queue_work([&]{ process(local); });

##### 糟糕的例子

    int local = 42;
    
    // 想要local的一个副本，因为有了local的副本，该副本都调用都是可用的
    thread_pool.queue_work([=]{ process(local); });

##### Enforcement


* (简单) 给出警告，如果捕获列表(capture-list)包含对本地声明的变量的引用。
* (复杂) 给出标记，当捕获列表包含对本地声明变量的引用，并且lambda被传递到非`const`和非本地上下文时。

### <a name="Rf-this-capture"></a>F.54: 如果你捕获了`this`，你就显式地捕获了所有变量(没有默认的捕获)

##### 原因

这是令人困惑的，在成员函数中使用`[=]`似乎是按值捕获的，但实际上是按引用捕获了成员对象，因为它实际上是按值捕获不可见的`this`指针，如果你打算这样做，把`this`写清楚。

##### 示例

    class My_class {
        int x = 0;
        // ...

        void f() {
            int i = 0;
            // ...

            auto lambda = [=]{ use(i, x); };   // BAD: “看起来像”按值捕获的
            // 在本规则下，[&]具有和拷贝this指针相同的语义，
            // [=,this] 和 [&,this]没有更好，也是令人困惑的

            x = 42;
            lambda(); // calls use(0, 42);
            x = 43;
            lambda(); // calls use(0, 43);

            // ...

            auto lambda2 = [i, this]{ use(i, x); }; // ok, 最明确，最不容易混淆

            // ...
        }
    };

##### Note

标准正对此进行了积极的讨论，在未来标准的版本中，可以通过添加新的捕获模式或调整`[=]`的含义来解决这个问题。现在，只需要明确。

##### Enforcement

* 标出任何指定默认捕获并捕获`this`(无论是通过显式还是默认捕获)的lambda捕获列表

### <a name="F-varargs"></a>F.55: 不要使用`va_arg`参数 

##### 原因

从`va_arg`读取数据时，会假定向函数传递了正确的类型；传递给`va_arg`时，会假定函数将读取正确的类型。
这是脆弱的，因为它通常不能在语言中强制执行以确保安全，因此会依赖于程序员规程来确保它的正确性。

##### 示例

    int sum(...) {
        // ...
        while (/*...*/)
            result += va_arg(list, int); // BAD, 假定它会传递整数列表
        // ...
    }

    sum(3, 2); // ok
    sum(3.14159, 2.71828); // BAD, 未定义的

    template<class ...Args>
    auto sum(Args... args) { // GOOD, 更灵活
        return (... + args); // note: C++17的折叠表达式(fold expression)
    }

    sum(3, 2); // ok: 5
    sum(3.14159, 2.71828); // ok: ~5.85987

##### 备选

* 重载
* 可变参数模板
* `variant`参数
* `initializer_list`(齐次的)

##### Note

声明一个`…`参数有时对不涉及实际参数传递的技术非常有用，特别是声明“获取任意类型参数”的函数，以便在重载的函数集中禁用“其它所有类型”的参数（译注：一般在重载的函数中处理无法使用`...`进行通用处理的类型），或者在模板元程序中表示包罗万象情况。

##### Enforcement

* 触发一个诊断(消息/提示)，如果使用了`va_list`、`va_start`、`va_arg`。
* 触发一个诊断(消息/提示)，如果将参数传递给了函数的`vararg`参数，但并没有在相应的地方提供该函数更具体类型的函数重载。修复方法：使用不同的函数，或者`[[suppress(types)]]`。

# <a name="S-class"></a>C: 类和类层次结构

类是用户定义的类型，程序员可以为其定义表示、操作和接口。
类层次结构用于将相关类组织成层次结构。

类规则的概述:

* [C.1: 将相关的数据组织成结构体(`struct`或`class`)](#Rc-org)
* [C.2: 如果具有不变式，则使用`class`；如果数据成员之间彼此独立，使用`struct`](#Rc-struct)
* [C.3: 使用类表示出接口和实现之间的区别](#Rc-interface)
* [C.4: 只有在函数需要直接访问类的表示时，才将其作为成员](#Rc-member)
* [C.5: 将辅助(helper)函数放在与其支持的类相同的名称空间中](#Rc-helper)
* [C.7: 不要在同一语句中定义类或枚举类型，同时又声明该类型的变量](#Rc-standalone)
* [C.8: 如果任何成员是非公共的，则使用`class`而不是`struct`](#Rc-class)
* [C.9: 最小化暴露类成员](#Rc-private)

子章节:

* [C.concrete: 具体类型](#SS-concrete)
* [C.ctor: 构造函数、赋值、析构函数](#S-ctor)
* [C.con: 容器和其它资源句柄](#SS-containers)
* [C.lambdas: 函数对象和lambda](#SS-lambdas)
* [C.hier: 类层次结构(OOP)](#SS-hier)
* [C.over: 重载和运算符重载](#SS-overload)
* [C.union: 联合体(union)](#SS-union)

### <a name="Rc-org"></a>C.1: 将相关的数据组织成结构体(`struct`或`class`)

##### 原因

易于理解，如果数据是相关的(出于基本原因)，那么这个事实应该反映在代码中。

##### 示例

    void draw(int x, int y, int x2, int y2);  // BAD: 不必要的隐式关系
    void draw(Point from, Point to);          // better

##### Note

没有虚函数的简单没有空间和时间的额外开销。

##### Note

从语言上，看`class`和`struct`的区别只在于它们成员的默认可见性的不同。

##### Enforcement

或许是不可能被强制的，启发式地寻找一起使用的数据项或许是可以的。

### <a name="Rc-struct"></a>C.2: 如果具有不变式，则使用`class`；如果数据成员之间彼此独立，使用`struct`

##### 原因

可读性；易于理解；`class`可以提醒程序员需要一个不变式；有用的惯例。

##### Note

不变式只是对象成员间的逻辑条件，构造函数必须为公共成员函数建立该逻辑条件。
一旦该不变式被建立(通常由构造函数)，该对象的成员函数都可以被调用。
不变式可以被非正式的表示(如，在注释中)，或者使用`Expects`更正式地说明。

如果所有的数据成员之间可以独立地变化，那就不存在不变式。

##### 示例

    struct Pair {  // 成员之间可以独立地变化
        string name;
        int volume;
    };

但是:

    class Date {
    public:
        // 验证{yy, mm, dd}是合法的，然后初始化
        Date(int yy, Month mm, char dd);
        // ...
    private:
        int y;
        Month m;
        char d;    // day
    };

##### Note

如果一个类有`private`数据，不使用构造函数，用法没办法完全初始化一个对象。
因此，类的定义都需要提供一个构造函数，并指明它的用意。
这实际上意味着类的定义者需要定义一个不变式。

**参见**:

* [将具有私有数据的类型定义为`class`](#Rc-class)
* [在类中先放置接口](#Rl-order)
* [最小化暴露类成员](#Rc-private)
* [避免受保护数据(protect)](#Rh-protected)

##### Enforcement

寻找都是私有数据的`struct`类型，以及都是公开成员的`class`类型。

### <a name="Rc-interface"></a>C.3: 使用类表示出接口和实现之间的区别

##### 原因

接口和实现之间的显示区别提高的可读性和可维护性

##### 示例

    class Date {
        // ... 一些表示 ...
    public:
        Date();
        // 验证{yy, mm, dd}是合法的，并初始化
        Date(int yy, Month mm, char dd);

        int day() const;
        Month month() const;
        // ...
    };

例如，我们可以改变`Date`的表示，但又不会影响到它的用户(可能需要重新编译)。

##### Note

表现接口和实现之间的区别，当然不只有使用类这一种方式，
例如，我们可以使用一组在相同命名空间独立声明的函数、抽象的基类、或者使用具有概念的模板函数来表示一个接口。
最重要的问题是显示式地区分接口和它的实现"细节"，
理想(通常)情况下，接口比它的实现要稳定得多。

##### Enforcement

???

### <a name="Rc-member"></a>C.4: 只有在函数需要直接访问类的表示时，才将其作为成员

##### Reason

成员函数间更少的耦合，修改对象而引起麻烦的函数更少，当类的表示发生变化时，需要修改的函数数量也更少。

##### Example

    class Date {
        // ... 相关的少量接口 ...
    };

    // 辅助函数(helper functions):
    Date next_weekday(Date);
    bool operator==(Date, Date);

辅助函数(helper function)没有必要直接访问`Date`的表示。

##### Note

如果C++有["uniform function call"](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0251r0.pdf)，这条规则会变得更好。

##### 例外

C++语言要求`virtual`函数作为成员，并不是所有的`virtual`函数都会直接访问数据，特别地，抽象类的成员函数很少这样做。

注意，[multi-methods](https://parasol.tamu.edu/~yuriys/papers/OMM10.pdf)

##### 例外

C++语言要求操作符`=`、`()`、`[]`和`->`是成员函数。

##### 例外

一组重载的成员函数中可能会有一些不直接访问私有数据：

    class Foobar {
    public:
        void foo(long x)    { /*操作私有数据*/ }
        void foo(double x) { foo(std::lround(x)); }
        // ...
    private:
        // ...
    };

##### 例外

类似地，一组成员函数可能被设计成在调用链中使用。

    x.scale(0.5).rotate(45).set_color(Color::red);

通常，只有一些(并非所有)函数会直接访问私有数据。

##### 实施

* 寻找非`virtual`且不直接接触数据成员的成员函数，问题在于，许多不需要直接接触数据成员的成员函数会这样做。
* 忽略`virtual`函数。
* 忽略作为重载集合中的函数，其中至少有一个函数访问私有(数据)成员。
* 忽略返回`this`的成员函数。

### <a name="Rc-helper"></a>C.5: 将辅助函数放在与其支持的类相同的名称空间中

##### Reason

辅助(helper)函数是一个不需要直接访问类的表示的函数(通常由类的编写者提供)，但被视为类的有用接口的一部分。
将它们放在与类相同的命名空间(namespace)中，可以使它们与类的关系变得更加明显，并允许通过依赖于参数的查找找到它们。

##### Example

    namespace Chrono { // 这里，我们提供时间相关的服务

        class Time { /* ... */ };
        class Date { /* ... */ };

        // 辅助函数：
        bool operator==(Date, Date);
        Date next_weekday(Date);
        // ...
    }

##### Note

这对[运算符重载](#Ro-namespace)特别重要。

##### Enforcement

* 标出只在一个namespace中获取参数的全局函数。

<!-- ### <a name="Rc-standalone"></a>C.7: Don't define a class or enum and declare a variable of its type in the same statement -->
### <a name="Rc-standalone"></a>C.7: 不要在同一语句中定义类或枚举类型，同时又声明该类型的变量

##### Reason

在同一个声明中混合类型定义和实体的定义是令人困惑的和不必要的。

##### 糟糕的例子

    struct Data { /*...*/ } data{ /*...*/ };

##### 好的例子

    struct Data { /*...*/ };
    Data data{ /*...*/ };

##### Enforcement

* 如果类或枚举定义的`}`后面不是跟着`;`，则标记它缺少`;`。

### <a name="Rc-class"></a>C.8: 如果任何成员是非公共的，则使用`class`而不是`struct`

##### Reason

可读性。
清楚地说明了某些东西被隐藏/抽象了。
这是一个有用的约定。

##### 糟糕的例子

    struct Date {
        int d, m;

        Date(int i, Month m);
        // ... 很多函数 ...
    private:
        int y;  // year
    };

如果只关心C++的语言规则，这段代码是没有什么问题的。
但是从设计的视角看，几乎所有都是有问题的：私有数据离公有数据太远了；在类的声明中，数据被分成了不同的部分；不同部分的数据具有不同的访问属性。
所有这些都降低了可读性，增加了维护的复杂性。

##### Note

倾向于在类中先放置接口，参见[NL.16](#Rl-order)。

##### Enforcement

标出声明为`struct`的类型，如果该类型有私有或受保护的成员。

### <a name="Rc-private"></a>C.9: 最小化暴露类成员

##### Reason

封装；
信息隐藏；
最小化意外的访问；
让维护更简单。

##### 示例

    template<typename T, typename U>
    struct pair {
        T a;
        U b;
        // ...
    };

Whatever we do in the `//`-part, an arbitrary user of a `pair` can arbitrarily and independently change its `a` and `b`.
In a large code base, we cannot easily find which code does what to the members of `pair`.
This may be exactly what we want, but if we want to enforce a relation among members, we need to make them `private`
and enforce that relation (invariant) through constructors and member functions.
For example:

无论我们在`//`部分怎么做，任何`pair`的用户都可以随意独立地修改`a`和`b`。
在一个大型的代码库中，很难定位到哪些代码对`pair`的成员变量做了什么。
这可能正是我们想要的，但是如果我们想要加强成员之间的关系，我们需要把它们变为`private`，并使用构造函数和成员函数来加强这种关系(不变式)。
例如：

    class Distance {
    public:
        // ...
        double meters() const { return magnitude*unit; }
        void set_unit(double u)
        {
                <!-- // ... check that u is a factor of 10 ... -->
                // ... 检查10是否为u的因数(译注：原文可能写反了)
                // ... 适当地改变大小 ...
                unit = u;
        }
        // ...
    private:
        double magnitude;
        double unit;    // 1是米，1000是千米， 0.001是毫米，等等
    };

##### Note

如果无法轻松确定一组变量的直接用户群，则无法(轻松)更改/改进该组变量的类型或用法。
公有和受保护的数据，通常就是这种情况。

##### Example

类可以提供两个接口给它的用户，一个给派生类(`protected`)，另一个给普通的用户(`public`)。
例如，一个派生类可能会允许跳过运行时检查，因为它已经保证了正确性。

    class Foo {
    public:
        int bar(int x) { check(x); return do_bar(x); }
        // ...
    protected:
        int do_bar(int x); // 在数据上做一些操作
        // ...
    private:
        // ... 数据 ...
    };

    class Dir : public Foo {
        //...
        int mem(int x, int y)
        {
            /* ... 做些什么 ... */
            return do_bar(x + y); // OK: 派生类可以绕过检查
        }
    };

    void user(Foo& x)
    {
        int r1 = x.bar(1);      // OK, 将会进行检查
        int r2 = x.do_bar(2);   // 错误: 会绕过检查(译注：可能会有编译错误，除非user是Foo的友元函数)
        // ...
    }

##### Note

[`protected`数据是一个糟糕的想法](#Rh-protected).

##### Note

选择这种放置顺序：首先是`public`成员，然后是`protected`成员，最后是`private`成员，参见[NL.16: 使用约定的类成员声明顺序](#Rl-order)

##### Enforcement

* 标出受保护的数据,参见[C.133: 避免使用受保护的数据](#Rh-protected)
* 标出混合使用`public`和`private`的类。

## <a name="SS-concrete"></a>C.concrete: 具体类型

一个理想的类应该常规(general)类型，这差不多是说其行为像一个`int`，具体类型是最简单的类。
常规类型的值是可以被复制的，并且复制的结果是与原始值相同的独立对象，如果一个具体类型同时具有`=`和`==`，那么`a = b`的结果应该会让`a == b`为`true`。
可以定义没有`=`和`==`的具体类，但是它们是(而且应该是)很少见的。
C++内置类型是常规的，标准库类也是常规的，比如`string`、`vector`和`map`。
具体类型通常也被称为值类型，以区别于用作层次结构一部分的类型。

具体类型规则概述：

* [C.10: 相较于类层次结构，优先使用具体类型](#Rc-concrete)
* [C.11: 让具体类型常规化](#Rc-regular)

### <a name="Rc-concrete"></a>C.10: 相较于类层次结构，优先使用具体类型

##### Reason

从根本上讲，具体类型给层次结构更简单、更容易设计、更容易实现、更容易使用、更容易推理、更小、更快。如果要用层次结构，你需要一个使用它的理由(用例)。

##### 示例

    class Point1 {
        int x, y;
        // ... operations ...
        // ... no virtual functions ...
    };

    class Point2 {
        int x, y;
        // ... operations, some virtual ...
        virtual ~Point2();
    };

    void use()
    {
        Point1 p11 {1, 2};   // make an object on the stack
        Point1 p12 {p11};    // a copy

        auto p21 = make_unique<Point2>(1, 2);   // make an object on the free store
        auto p22 = p21->clone();                // make a copy
        // ...
    }

如果一个类可以是层次结构的一部分，那么我们(在实际代码中，而不是在小的示例中)必须通过指针或引用操作它的对象。
这意味着需要更多的内存开销、更多的分配和释放，以及更多的运行时开销来执行产生的间接操作。

##### Note

具体类型可以是堆栈分配的，也可以是其他类的成员。

##### Note

间接使用主要是为了接口运行时多态，而不是分配和释放的开销(这只是最常见的情况)
我们可以使用基类作为作用域内派生类对象的接口。
这是在禁止动态分配的情况下完成的(例如，硬实时)，并为某些插件提供稳定的接口。

##### Enforcement

???

### <a name="Rc-regular"></a>C.11: 让具体类型常规化

##### Reason

常规类型比非常规类型更容易理解和推理，非常规类型需要额外的努力来理解和使用。

##### Example

    struct Bundle {
        string name;
        vector<Record> vr;
    };

    bool operator==(const Bundle& a, const Bundle& b)
    {
        return a.name == b.name && a.vr == b.vr;
    }

    Bundle b1 { "my bundle", {r1, r2, r3}};
    Bundle b2 = b1;
    if (!(b1 == b2)) error("impossible!");
    b2.name = "the other bundle";
    if (b1 == b2) error("No!");

特别地，如果一个具体类型能被赋值，也要给它一个等号运算符，这样`a = b`意味着`a == b`。

##### Note

资源句柄是不能被复制的，例如`mutex`的`scoped_lock`，与具体类型类似，它们通常是基于堆分配的。
然而，由于这些类型通常是不能被复制的(相反，它们可以被move)，因此它们不是常规类型，不过，它们往往是"半常规的"。
通常，这些类型被当作是"仅可移动的类型"。

##### Enforcement

???

## <a name="S-ctor"></a>C.ctor: 构造函数、赋值和析构函数

这些函数控制了对象的生命周期：创建、复制、移动和销毁。
通过定义构造函数来保证和简化类的初始化。

这些是*默认操作*：

* 默认构造函数: `X()`
* 拷贝构造函数: `X(const X&)`
* 拷贝赋值: `operator=(const X&)`
* 移动构造函数: `X(X&&)`
* 移动赋值: `operator=(X&&)`
* 析构函数: `~X()`

默认情况下，如果使用这些操作，编译器将定义它们中的每一个，但是默认操作可以被取消。
默认操作是一组相关的操作，它们共同实现对象的生命周期语义。
默认情况下，C++将类视为值类型，但并非所有类型都是值类型。

默认操作规则：

* [C.20: 尽量避免定义任何默认操作](#Rc-zero)
* [C.21: 如果你定义或`=delete`了任何默认操作，把所有默认操作都定义或`=delete`掉](#Rc-five)
* [C.22: 让默认操作保持一致](#Rc-matched)

析构函数规则:

* [C.30: 定义析构函数，如果类需要显示地销毁对象](#Rc-dtor)
* [C.31: 类所获取的所有资源，都需由析构函数来释放](#Rc-dtor-release)
* [C.32: 如果类具有原始指针(`T*`)或引用(`T&`)，考虑它是否有所有权 ](#Rc-dtor-ptr)
* [C.33: 如果类有一个具有所有权的指针成员，定义或`=delete`一个析构函数](#Rc-dtor-ptr2)
* [C.35: 基类的析构函数应当要么是public和virtual的，要么是protect和非virtual的](#Rc-dtor-virtual)
* [C.36: 析构函数应当不会失败](#Rc-dtor-fail)
* [C.37: 让析构函数`noexcept`](#Rc-dtor-noexcept)

构造函数规则:

* [C.40: 如果类具有不变式，定义一个构造函数](#Rc-ctor)
* [C.41: 构造函数应当创建一个完全初始化的对象](#Rc-complete)
* [C.42: 抛出异常，如果构造函数不能构造出合法的对象](#Rc-throw)
* [C.43: 确保可拷贝的类型(值类型)有一个默认构造函数](#Rc-default0)
* [C.44: 优先使用简单而没有异常的默认构造函数](#Rc-default00)
* [C.45: Don't define a default constructor that only initializes data members; use member initializers instead](#Rc-default)
* [C.46: By default, declare single-argument constructors `explicit`](#Rc-explicit)
* [C.47: Define and initialize member variables in the order of member declaration](#Rc-order)
* [C.48: Prefer in-class initializers to member initializers in constructors for constant initializers](#Rc-in-class-initializer)
* [C.49: Prefer initialization to assignment in constructors](#Rc-initialize)
* [C.50: Use a factory function if you need "virtual behavior" during initialization](#Rc-factory)
* [C.51: Use delegating constructors to represent common actions for all constructors of a class](#Rc-delegating)
* [C.52: Use inheriting constructors to import constructors into a derived class that does not need further explicit initialization](#Rc-inheriting)

Copy and move rules:

* [C.60: Make copy assignment non-`virtual`, take the parameter by `const&`, and return by non-`const&`](#Rc-copy-assignment)
* [C.61: A copy operation should copy](#Rc-copy-semantic)
* [C.62: Make copy assignment safe for self-assignment](#Rc-copy-self)
* [C.63: Make move assignment non-`virtual`, take the parameter by `&&`, and return by non-`const&`](#Rc-move-assignment)
* [C.64: A move operation should move and leave its source in a valid state](#Rc-move-semantic)
* [C.65: Make move assignment safe for self-assignment](#Rc-move-self)
* [C.66: Make move operations `noexcept`](#Rc-move-noexcept)
* [C.67: A polymorphic class should suppress copying](#Rc-copy-virtual)

Other default operations rules:

* [C.80: Use `=default` if you have to be explicit about using the default semantics](#Rc-eqdefault)
* [C.81: Use `=delete` when you want to disable default behavior (without wanting an alternative)](#Rc-delete)
* [C.82: Don't call virtual functions in constructors and destructors](#Rc-ctor-virtual)
* [C.83: For value-like types, consider providing a `noexcept` swap function](#Rc-swap)
* [C.84: A `swap` may not fail](#Rc-swap-fail)
* [C.85: Make `swap` `noexcept`](#Rc-swap-noexcept)
* [C.86: Make `==` symmetric with respect of operand types and `noexcept`](#Rc-eq)
* [C.87: Beware of `==` on base classes](#Rc-eq-base)
* [C.89: Make a `hash` `noexcept`](#Rc-hash)

## <a name="SS-defop"></a>C.defop: Default Operations

By default, the language supplies the default operations with their default semantics.
However, a programmer can disable or replace these defaults.

### <a name="Rc-zero"></a>C.20: 尽量避免定义任何默认操作

##### Reason

最简单，且提供了最干净的语义。

##### Example

    struct Named_map {
    public:
        // ... 没有默认操作的声明 ...
    private:
        string name;
        map<int, int> rep;
    };

    Named_map nm;        // 默认构造
    Named_map nm2 {nm};  // 拷贝构造

既然`std::map`和`string`已经有了所有的特殊函数，因此没有更多工作要做。

##### Note

这就是所谓的"零原则"("the rule of zero")。(译注：参见[Rule of Zero](https://isocpp.org/blog/2012/11/rule-of-zero)）

##### Enforcement

(不可强制执行) 虽然不可强制执行，但是一个好的静态分析器可以检测出可能满足这个规则的改进模式。
例如，一个类有一对成员{指针，大小}和一个析构函数，析构函数中对指针的`delete`操作可能可以转换成`vector`(译注：这里说也就是所谓的`RAII`，`vector`可以自己管理内存)。

### <a name="Rc-five"></a>C.21: 如果你定义或`=delete`了任何默认操作，把所有默认操作都定义或`=delete`掉

##### Reason

*特殊成员函数*指默认构造函数、拷贝构造函数、拷贝赋值运算符、移动构造函数、移动赋值运算符、以及析构函数。

特殊函数的语义密切相关，因此如果需要声明一个函数，那么其他函数也需要考虑。

声明除默认构造函数外的任何特殊成员函数，即使是`=default`或`=delete`，也会禁止隐式声明移动构造函数和移动赋值操作符。
声明一个移动构造函数或移动赋值操作符，甚至`=default`或`=delete`，也将导致隐式生成的拷贝构造函数和拷贝赋值运算符被定义为已删除。
因此，一旦任何一个特殊函数被声明，其他函数就应该也都被声明，以避免不必要的影响，如将潜在的移动操作变成更昂贵的拷贝，或可仅移动的类型。

##### 糟糕的例子

    struct M2 {   // 糟糕：不完整的默认操作
    public:
        // ...
        // ... 没有拷贝或移动操作 ...
        ~M2() { delete[] rep; }
    private:
        pair<int, int>* rep;  // 以0结束的pair集合
    };

    void use()
    {
        M2 x;
        M2 y;
        // ...
        x = y;   // 默认赋值操作
        // ...
    }

格外注意析构函数(这里用来释放)，拷贝赋值和移动赋值(两者都隐式地销毁对象)正确执行的可能性很低(这里，我们会得到两次delete)。
(译注：这里使用了浅复制(shadow copy)，因此x和y销毁时都会都释放rep指向的同一块内存)

##### Note

所谓的"the rule of five"或"the rule of six"，取决于你是否算上默认构造函数。
(译注：`默认构造函数、`拷贝构造函数、拷贝赋值运算符、移动构造函数、移动赋值运算符、以及析构函数)

##### Note

在定义其它操作时，你希望默认操作能有默认实现，使用`=default`，以显示你确实想这样做。如果你不想要一个默认操作，使用`=delete`来抑制(编译器生成)。

##### 糟糕的例子

如果析构函数只需要被声明成`virtual`，也可以定义成默认的。为了避免抑制隐式的移动操作，它们必须被声明。然后，为了避免类变成"仅可移动"的(不能被复制)，拷贝操作也必须被声明。

    class AbstractBase {
    public:
      virtual ~AbstractBase() = default;
      AbstractBase(const AbstractBase&) = default;
      AbstractBase& operator=(const AbstractBase&) = default;
      AbstractBase(AbstractBase&&) = default;
      AbstractBase& operator=(AbstractBase&&) = default;
    };

或者按照[C.67](#Rc-copy-virtual)防止切片，可以删除拷贝和移动操作:

    class ClonableBase {
    public:
      virtual unique_ptr<ClonableBase> clone() const;
      virtual ~ClonableBase() = default;
      ClonableBase(const ClonableBase&) = delete;
      ClonableBase& operator=(const ClonableBase&) = delete;
      ClonableBase(ClonableBase&&) = delete;
      ClonableBase& operator=(ClonableBase&&) = delete;
    };

在这里，仅定义移动操作或仅定义拷贝操作将具有相同的效果，但是，显式地说明每个特殊成员函数的意图，会让读者更容易理解。

##### Note

编译器强制执行这条规则的大部分内容，理想情况下，编译器会警告任何违反这些规则的情况。

##### Note

具有析构函数的类不再隐式生成拷贝操作。

##### Note

写六个特殊成员函数可能是易于出错的，注意他们的参数类型：

    class X {
    public:
        // ...
        virtual ~X() = default;            // 析构函数(virtual，如果X要作为基类)
        X(const X&) = default;             // 拷贝构造函数
        X& operator=(const X&) = default;  // 拷贝赋值
        X(X&&) = default;                  // 移动构造函数
        X& operator=(X&&) = default;       // 移动赋值
    };

一个小失误(例如，拼写错误、遗漏`const`、使用`&`而非`&&`、或者遗漏了一个特殊函数)可能导致错误或警告。为了避免单调和错误的可能性，试着遵循[rule of zero](#Rc-zero)。

##### Enforcement

(简单) 一个类应当，要么声明(甚至使用`=delete`)所有的特殊函数，要么一个都不声明。

### <a name="Rc-matched"></a>C.22: 让默认操作保持一致

##### Reason

默认操作是一组在概念上相匹配的集合，它们的语义是相互关联的。
如果拷贝/移动构造函数和复制/移动赋值函数在做逻辑上不同的事情，用户会感到惊讶；如果构造函数和析构函数不能提供资源管理的一致视图，用户会感到惊讶；如果拷贝和移动没有反映构造函数和析构函数的工作方式，用户会感到惊讶。

##### Example, bad

    class Silly {   // 糟糕：不一致的拷贝操作
        class Impl {
            // ...
        };
        shared_ptr<Impl> p;
    public:
        Silly(const Silly& a) : p{a.p} { *p = *a.p; }   // 深拷贝(deep copy)
        Silly& operator=(const Silly& a) { p = a.p; }   // 浅拷贝(shallow copy)
        // ...
    };

这些操作在拷贝语义上存在分歧，这将导致混乱和错误。

##### Enforcement

* (复杂)拷贝/移动构造函数及其相应的操作符应该在相同的解引用级别写入相同的成员变量。
* (复杂)拷贝/移动构造函数中写入的任何成员变量也应该由所有其他构造函数初始化。
* (复杂)如果拷贝/移动构造函数执行成员变量的深拷贝(deep copy)，那么析构函数也应该修(清理)改该成员变量。
* (复杂)如果析构函数修改(清理)成员变量，那么该成员变量应该在所有的拷贝/移动构造函数或赋值操作符中被写入。

## <a name="SS-dtor"></a>C.dtor: 析构函数

该类确实需要析构函数吗？是一个异常强大的设计问题。
对于大多数类而言，答案是否定的，要么是因为该类没有持有资源，要么是因为销毁是由[the rule of zero](#Rc-zero)处理的。
那也就是说，它们的成员变量可以处理他们自己的销毁。
如果答案是肯定的，即确实需要一个析构函数，该类大部的设计都要遵循[the rule of five](#Rc-five)。

### <a name="Rc-dtor"></a>C.30: 定义析构函数，如果类需要显示地销毁对象

##### Reason

析构函数会在对象生命周期结束后被隐式地调用，如果默认析构函数已经足够，就使用它。
只有在类需要执行不属于其成员对象析构函数一部分的代码时，才定义非默认的析构函数。

##### Example

    template<typename A>
    struct final_action {   // 稍微被简化了
        A act;
        final_action(A a) :act{a} {}
        ~final_action() { act(); }
    };

    template<typename A>
    final_action<A> finally(A act)   // 推断action的类型
    {
        return final_action<A>{act};
    }

    void test()
    {
        auto act = finally([]{ cout << "Exit test\n"; });  // 建立退出action
        // ...
        if (something) return;   // 这里，act已结束
        // ...
    } // 这里，act已结束

`final_action`的目的是让一段代码(通常是lambda)能在销毁之前执行。

##### Note

有两类类需要用户定义的析构函数：

* 具有资源的类，而资源本身不是具有析构函数的类，例如，`vector`或事务类。
* 主要用于在对象销毁时执行操作的类，如跟踪程序或`final_action`。

##### 糟糕的例子

    class Foo {   // 糟糕，使用默认的析构函数即可
    public:
        // ...
        ~Foo() { s = ""; i = 0; vi.clear(); }  // 清理操作
    private:
        string s;
        int i;
        vector<int> vi;
    };

默认析构函数可以做的更好，更有效率，并且也不会出错。

##### Note

如果需要默认构造函数，但它的生成被抑制了(如，通过定义移动构造函数)，使用`=default`。

##### Enforcement

寻找看起来"隐式的资源"，例如指针和引用。寻找所有成员对象都有析构函数，但又定义了析构函数的类。

### <a name="Rc-dtor-release"></a>C.31: 类所获取的所有资源，都需由析构函数来释放

##### Reason

防止资源泄漏，特别是在错误的情况下。

##### Note

对于由具有完整默认操作的类表示的资源，这将自动发生。（译注：这里的类指的是具有五个默认特殊函数的类）

##### Example

    class X {
        ifstream f;   // 可能持有了一个文件
        // ... 没有定义默认操作，或使用了`=deleted` ... 
    };

在`X`销毁时，`ifstream`会隐式地关闭它所打开的所有文件。

##### 糟糕的例子

    class X2 {     // bad
        FILE* f;   // 可能持有了一个文件
        // ... 没有定义默认操作，或使用了`=deleted`...
    };

`X2`可能漏泄了一个文件句柄。

##### Note

一个不会关闭的socket会怎么样？析构函数、close、或清理操作[应当不会失败](#Rc-dtor-fail)，
然而，如果这确实发生，我们就有了一个没有真正好的解决方案的问题。
对亲手而言，析构函数的编写者不知道为什么要调用析构函数，也不会通过抛出异常来“拒绝执行”，参见[讨论](#Sd-never-fail)。
让这个问题更加糟糕很多的“关闭/释放”操作不会重试，
许多人试图解决这个问题，但没有一个通用的解决方案，
如果可能的话，考虑关闭/清除失败是一个基本的设计错误并退出程序。

##### Note

一个类可以持有不属于它的对象的指针和引用，显然，这些对象不应该由该类的析构函数进行释放，例如：

    Preprocessor pp { /* ... */ };
    Parser p { pp, /* ... */ };
    Type_checker tc { p, /* ... */ };

这里，`p`引用了`pp`，但`pp`并不属于它。

##### Enforcement

* (简单) 如果一个类有其它所有者(如使用`gsl::owner`表示的所有者)的指针或引用成员变量，那么在它的析构函数中也应该是引用着的。
* (困难) 当没有显式的所有权声明时，确定指针或引用成员变量是否是所有者(例如，查看构造函数)。

<!-- If a class has a raw pointer (`T*`) or reference (`T&`), consider whether it might be owning -->
### <a name="Rc-dtor-ptr"></a>C.32: 如果类具有原始指针(`T*`)或引用(`T&`)，考虑它是否有所有权 

##### Reason

有非常多的代码与所有权无关。

##### Example

    ???

##### Note

如果`T*`或`T&`有所有权，标记为`owning`，否则，考虑将其标记为`ptr`。这是为了批注(documentation)及分析。

##### Enforcement

查看原始成员指针(raw member pointer)和成员引用的初始化，并查看是否使用了分配(allocation)。

### <a name="Rc-dtor-ptr2"></a>C.33: 如果类有一个具有所有权的指针成员，定义或`=delete`一个析构函数

##### Reason

被持有的对象必须在被(持有其的对象)销毁时被`删除`。

##### Example

一个指针成员可能代表了一个资源，[`T*`不应该这样做](#Rr-ptr)，但在老代码中，这很常见。
让`T*`做为一个可能的持有者值这一做法是值得怀疑的。

    template<typename T>
    class Smart_ptr {
        T* p;   // 糟糕: 模糊了*p的所有权
        // ...
    public:
        // ... 没有用户定义的默认操作 ...
    };

    void use(Smart_ptr<int> p1)
    {
        // 错误：p2.p被泄漏了(如果p2不是nullptr，且不被其它代码持有)
        auto p2 = p1;
    }

Note that if you define a destructor, you must define or delete [all default operations](#Rc-five):
注意，如果你定义的析构函数，你需要定义或删除[所有的默认操作](#Rc-five)：

    template<typename T>
    class Smart_ptr2 {
        T* p;   // 糟糕: 模糊了*p的所有权
        // ...
    public:
        // ... 没有用户定义的默认操作  ...
        ~Smart_ptr2() { delete p; }  // p是一个所有者！
    };

    void use(Smart_ptr2<int> p1)
    {
        auto p2 = p1;   // 错误：两次delete (译注：使用浅拷贝(shadow copy))
    }

The default copy operation will just copy the `p1.p` into `p2.p` leading to a double destruction of `p1.p`. Be explicit about ownership:
默认的拷贝操作仅仅将`p1.p`拷贝到`p2.p`，导致了两次销毁`p1.p`。明确所有权：

    template<typename T>
    class Smart_ptr3 {
        owner<T*> p;   // OK: 明确了*p的所有权
        // ...
    public:
        // ...
        // ... 拷贝和移动操作 ...
        ~Smart_ptr3() { delete p; }
    };

    void use(Smart_ptr3<int> p1)
    {
        auto p2 = p1;   // OK: 没有两次删除
    }

##### Note

通常，获得析构函数最简单的方式是将指针替换成智能指针(如`std::unique_ptr`)，让编译来适当地处理销毁。

##### Note

为什么不要求全部的所有指针都是智能指针？
这有时需要更改非平凡(non-trivial)代码，这可能会影响到ABI。

##### Enforcement

* 具有指针数据成员的类是可疑的。
* 具有`owner<T>`的类需要定义它的默认操作。


### <a name="Rc-dtor-virtual"></a>C.35: 基类的析构函数应当要么是public和virtual的，要么是protect和非virtual的

##### Reason

为了防止未定义的行为。
如果析构函数是公开的，那么可以通过基类的指针来销毁派生类对象，如果基类的析构函数不是virtual的，那么结果是未定义的。如果析构函数是受保护的，那么无法通过基类的指针来销毁派生类对象，所以析构函数不需要是virtual。析构函数不能是私有的(private)，这样派生类的析构函数可以调用它。
通常，一个基类的作者不知道在销毁时要做的适当工作。

##### Discussion

参见 [this in the Discussion section](#Sd-dtor).

##### Example, bad

    struct Base {  // 糟糕：隐含的公有的非virtual析构函数
        virtual void f();
    };

    struct D : Base {
        string s {"a resource needing cleanup"};
        ~D() { /* ... 做事清理工作 ... */ }
        // ...
    };

    void use()
    {
        unique_ptr<Base> p = make_unique<D>();
        // ...
    } // p的销毁调用了~Base()，而不是~D()，泄漏了D::s或者更多。

##### Note

虚函数定义的派生类接口，可以不通过派生类来调用它。如果接口允许销毁(对象)，它应该是安全的。

##### Note

析构函数应该是非私有的，否则会阻止该类型(该类定义的类型)的使用。

    class X {
        ~X();   // 私有的析构函数
        // ...
    };

    void use()
    {
        X a;                        // 错误：不能销毁
        auto p = make_unique<X>();  // 错误：不能销毁
    }

##### Exception

我们可以想象一种情况,你可能想要一个受保护的虚拟析构函数：当一个派生类对象(不是基类本身的对象)应当允许通过*基类指针*销毁*另一个*对象(不是其本身)，尽管我们没有在实践中见过这种情况。
*（译注：由于基类的析构函数是受保护的，因此无法直接通过基类指针来销毁对象，但却可以在派生的析构函数中通过基类指针来销毁其它对象，这是由于受保护的基类接口可以在派生类中被调用）*


##### Enforcement

* 任何一个具有虚函数的类都应当有一个公有虚拟的析构函数，或一个受保护的非虚析构函数。

### <a name="Rc-dtor-fail"></a>C.36: 析构函数应当不会失败

##### Reason

通常，如果析构函数会失败，我们不知道如何写没有错误的代码。
标准库要求它所处理的类的析构函数不会通过抛出异常来退出。

##### Example

    class X {
    public:
        ~X() noexcept;
        // ...
    };

    X::~X() noexcept
    {
        // ...
        if (cannot_release_a_resource) terminate();
        // ...
    }

##### Note

很多试图设计过能万无一失地处理析构函数中失败的方案，但没有人能成功地想出一个通用方案。
这可能是一个真正的实际问题：例如，一个不会关闭的套接字(socket)，析构函数的作者不知道为什么要调用析构函数，以及为什么不能抛出异常来“拒绝执行”，参见[讨论](#Sd-dtor)。
使问题更糟糕的是，很多“关闭/释放”操作是不能重试的(retryable)。 如果可能的话，将”关闭/清理“操作的失败看成是设计的缺陷，(失败发生后直接)退出程序。

##### Note

将析构函数声明为`noexcept`，可以保证它要么正常完成，要么退出程序。

##### Note

如果资源无法释放，程序可能不会失败，则尝试以某种方式向系统的其他部分发出失败信号(也许，甚至可以通过修改一些全局状态来让某些逻辑注意到并能够处理这个问题)，
要充分认识到这种技术是专用的，并且容易出错。
考虑“我的连接不会关闭”的例子，
可能在连接的另一端有一个问题，只有负责连接两端的代码(逻辑)可以正确地处理这个问题，
析构函数可以(以某种方式)向负责的系统发送一条消息，并认为它已经(成功)关闭了连接，然后正常返回。

##### Note

如果析构函数执行了一些可能会失败的操作，它可以捕获异常并且在一些情况下可以成功完成
（例如，通过使用与抛出异常的对象不同的清理机制）。

##### Enforcement

(简单) 如果析构函数会抛出异常，将其声明为`noexcept`。

### <a name="Rc-dtor-noexcept"></a>C.37: 让析构函数`noexcept`

##### Reason

 [析构函数应当不会失败](#Rc-dtor-fail)，如果析构函数退出时有异常，那么这将是一个糟糕的设计错误，程序最好终止。

##### Note

如果类的所有成员的析构函数都是`noexcept`的，则析构函数(用户定义的或编译器生成的)将被隐式地声明为`noexcept`(独立于其主体中的代码)。通过显式地标记析构函数为`noexcept`，作者可以防止通过添加或修改类成员而隐式地将析构函数变为`noexcept(false)`。

##### Example

默认情况下，并非所有析构函数都是不抛出异常的；一个抛出异常的成员会破坏整个类层次结构：

    struct X {
        Details x;  // 碰巧具有抛出异常的析构函数
        // ...
        ~X() { }    // 隐式的noexcept(false)，也就是说，可以抛出异常
    };

所以，如果有疑惑，将析构函数声明为`noexcept`。

##### Note

那么，为什么不将所有的析构函数都声明为`noexcept`呢？因为在很多情况下，特别是一些简单的情况，这会分散注意力。

##### Enforcement

(简单) 如果一个析构函数会抛出异常，那么它应该被声明为`noexcept`。

## <a name="SS-ctor"></a>C.ctor: 构造函数

构造函数定义了一个对象如何被初始化(构造)。

### <a name="Rc-ctor"></a>C.40: 如果类具有不变式，定义一个构造函数

##### Reason

这就是构造函数的作用。

##### Example

    class Date {  // a Date represents a valid date Date代表一个在1900年1月1和2100年12月31日之间的合法的日期
        Date(int dd, int mm, int yy)
            :d{dd}, m{mm}, y{yy}
        {
            if (!is_valid(d, m, y)) throw Bad_date{};  // 强制一个不变式
        }
        // ...
    private:
        int d, m, y;
    };

在构造函数中使用`Ensures`来表达不变式通常是一个好的想法。

##### Note

即使类没有不变式，也可以用构造函数以方便使用，例如:

    struct Rec {
        string s;
        int i {0};
        Rec(const string& ss) : s{ss} {}
        Rec(int ii) :i{ii} {}
    };

    Rec r1 {7};
    Rec r2 {"Foo bar"};

##### Note

C++11的初始化列表可以消除对许多构造函数的需求，例如：

    struct Rec2{
        string s;
        int i;
        Rec2(const string& ss, int ii = 0) :s{ss}, i{ii} {}   // 冗余的
    };

    Rec2 r1 {"Foo", 7};
    Rec2 r2 {"Bar"};

`Rec2`的构造函数是冗余的，同样，`int`的默认值作为[成员初始化器](#Rc-in-class-initializer)会更好。

**参见**: [构造合法的对象](#Rc-complete) 和 [构造函数异常抛出](#Rc-throw).

##### Enforcement

* 标出含有用户定义的拷贝操作但没有构造函数的类(用户定义的拷贝是该类具有不变式的良好标识)。

### <a name="Rc-complete"></a>C.41: 构造函数应当创建一个完全初始化的对象

##### Reason

构造函数为类建立不变式，类的用户应该能够假定构好的对象是可用的。

##### Example, bad

    class X1 {
        FILE* f;   // 在调用任何函数之前先调用init()
        // ...
    public:
        X1() {}
        void init();   // 初始化f
        void read();   // 从f中读数据
        // ...
    };

    void f()
    {
        X1 file;
        file.read();   // crash或读了错误数据
        // ...
        file.init();   // 太迟了
        // ...
    }

编译器不会阅读注释。

##### 例外

如果一个合法的对象不能由构造函数方便地构造出来，参见[使用工厂方法](#Rc-factory)。

##### Enforcement

* (简单) 每个构造函数都应该初始化所有的成员变量(显示式地通过委托构造函数或默认构造函数)。
* (未知) 如果构造函数有一个`Ensures`契约，试着查看它是否也作为后置条件。

##### Note

如果构造函数获得了一个资源(创建了一个合法的对象)，那么，该资源应该[由构造函数进行释放](#Rc-dtor-release)。
由构造函数获取资源，并由析构函数释放资源的习惯用法称为[RAII](#Rr-raii)(“资源获取即初始化”)。

### <a name="Rc-throw"></a>C.42: 抛出异常，如果构造函数不能构造出合法的对象

##### Reason

留下一个不合法的对象会引来麻烦。

##### Example

    class X2 {
        FILE* f;
        // ...
    public:
        X2(const string& name)
            :f{fopen(name.c_str(), "r")}
        {
            if (!f) throw runtime_error{"could not open" + name};
            // ...
        }

        void read();      // 从f中读数据
        // ...
    };

    void f()
    {
        X2 file {"Zeno"}; // 如果file没有打开，抛出异常
        file.read();      // fine
        // ...
    }

##### 糟糕的示例

    class X3 {     // bad: 构造函数给后面留下了一个不合法的对象
        FILE* f;   // 在调用任何函数之前，先调用is_valid()
        bool valid;
        // ...
    public:
        X3(const string& name)
            :f{fopen(name.c_str(), "r")}, valid{false}
        {
            if (f) valid = true;
            // ...
        }

        bool is_valid() { return valid; }
        void read();   // 从f中读数据
        // ...
    };

    void f()
    {
        X3 file {"Heraclides"};
        file.read();   // crash或读了错误数据
        // ...
        if (file.is_valid()) {
            file.read();
            // ...
        }
        else {
            // ... 错误处理 ...
        }
        // ...
    }

##### Note

对于变量的定义(如，在栈上或作为其它对象的成员)，没有能返回错误代码的显式函数调用，留下的非法对象依赖于用户在使用之前都要检查`is_valid()`函数，这种检查是乏味的、易于出错的、没有效率的。

##### 例外

有一些领域，从时间的角度来看，异常处理是低效的，如一些硬实时系统(想像一下飞机控制)。
这里必须使用`is_valid()`这一技术，在这种情况下，可以通过始终如一地即时检查`is_valid()`来模拟[RAII](#Rr-raii)。

##### 可选方案

如果你对“构造后初始化”或“两阶段初始化”有兴趣，尝试不要这样做，如果真的有必要这样做，参考[工厂方法](#Rc-factory)。
*译注：“构造后初始化”或“两阶段初始化”是指对象创建之后，调用一些类似init的方法来初始化对象。*

##### Note

人们使用`init`方法而不是在构造函数中执行初始化工作的其中一个原因是避免重复代码，[委托构造函数](#Rc-delegating)和[成员变量默认初始化](#Rc-in-class-initializer)可以做的更好。
另一个原因是延迟初到对象需要时才初始化，这种情况的解决方法经常是[不要声明一个变量，直到它可以被正确地初始化](#Res-init)。

##### Enforcement

???

### <a name="Rc-default0"></a>C.43: 确保可拷贝的类型(值类型)有一个默认构造函数

##### Reason

许多语言和库设施依赖默认构造函数来初始化自己的元素，例如，`T a[10]`和`std::vector<T> v(10)`。
默认构造函数通常可以让定义一个可拷贝的类型变得简单，参见[moved-from state](#???)。

##### Note

[值类型](#SS-concrete)是一种可以拷贝(通常也能比较)的类，它也和普通类型的概念密切相关，参见[EoP](http://elementsofprogramming.com/)和[the Palo Alto TR](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2012/n3351.pdf)

##### Example

    class Date { // BAD: 没有默认构造函数
    public:
        Date(int dd, int mm, int yyyy);
        // ...
    };

    vector<Date> vd1(1000);   // 这里需要Date的默认构造函数
    vector<Date> vd2(1000, Date{Month::October, 7, 1885});   // 可选方案

默认构造函数只有在没有声明的构造函数时才会(由编译器)自动生成，因此，在上述例子中不能初始化`vd1`。
默认构造函数的缺失可能让用户感到吃惊，并将使用复杂化，所以如果有合适的理由就应该定义它。

选择`Date`是为了鼓励思考：
没有”自然的“默认日期(宇宙大爆炸对大多数人来说太过遥远而没有用处)，所以这个例子是非平凡的(non-trivial)。
`{0, 0, 0}`在大多数的日历系统中都是非法的，所以选择这个值类似于引入了浮点数的`NaN`。
然而，大多数的`Date`类都有”第一天“(例如，1970/1/1就很常见)，所以选择这一天作为默认值是无关紧要的。

    class Date {
    public:
        Date(int dd, int mm, int yyyy);
        Date() = default; // [参见](#Rc-default)
        // ...
    private:
        int dd = 1;
        int mm = 1;
        int yyyy = 1970;
        // ...
    };

    vector<Date> vd1(1000);

##### Note

所有成员都具有默认构造函数的类隐式地获得一个默认构造函数：

    struct X {
        string s;
        vector<int> v;
    };

    X x; // means X{{}, {}}; that is the empty string and the empty vector

Beware that built-in types are not properly default constructed:

    struct X {
        string s;
        int i;
    };

    void f()
    {
        X x;    // x.s is initialized to the empty string; x.i is uninitialized

        cout << x.s << ' ' << x.i << '\n';
        ++x.i;
    }

静态分配的内置类型对象默认初始化成`0`，但内置类型的局部变量不会(默认初始化)。
注意，你的编译器可能会默认初始化局部的内置类型变量，然而，优化的构建却不会(译注：如打开-O2优化时)。
因此，类似上面示例中的代码看起来可能会工作，但这依赖于未定义的行为。
假设你想要初始化，一个显式的默认初始化可以提供帮助：

    struct X {
        string s;
        int i {};   // 默认初始化成0
    };

##### Notes

没有合理的默认构造的类通常也不可拷贝，所以它们不属于这个准则，
例如，基类不属于值类型(基类不应该能拷贝)，所以不需要一个默认构造函数：

    // Shape是一个抽象的基类，不是一个可拷贝的值类型。
    // 它可能需要，也可能不需要一个默认构造函数。
    struct Shape {
        virtual void draw() = 0;
        virtual void rotate(int) = 0;
        // =delete 拷贝/移动操作
        // ...
    };

在构造过程中必须获得调用者提供的资源的类通常不能有默认的构造函数，但是它不属于这个准则，因为这样的类通常是不可拷贝的:

    // std::lock_guard不是一个可拷贝的值类型，它没有一个默认构造函数
    lock_guard g {mx};  // guard the mutex mx 
    lock_guard g2;      // error: guarding nothing

拥有“特殊状态”的类必须由成员函数或使用者与其他状态分开处理，这将导致额外的工作，而且很可能出现更多错误。这种类型可以自然地使用特殊状态作为默认构造的值，不管它是否可复制:

    // std::ofstream是不可拷贝的值类型。
    // 它碰巧有一个默认构造函数，附带一下“不可打开”的特殊状态。
    ofstream out {"Foobar"};
    // ...
    out << log(time, transaction);

类似的可拷贝的特殊状态类型，比如，可拷贝的智能指针具有特殊状态"==nullptr"，应该使用特殊状态作为它们的默认构造值。

但是，最好让默认构造函数将其默认为有意义的状态，例如，`std::string`默认为`""`和`std::vector`默认为`{}`。

##### Enforcement

* 标出能使用`=`进行拷贝，但没有默认构造函数的类。
* 标出能使用`==`进行比较，但不能拷贝的类。

### <a name="Rc-default00"></a>C.44: 优先使用简单且没异常抛出的默认构造函数

##### Reason

能够通过使用不可能失败的操作将值设为“默认值”，这简化了有关移动操作的错误处理和推理。

##### 有问题的示例

    template<typename T>
    // elem指向使用new分配的space-elem元素
    class Vector0 {
    public:
        Vector0() :Vector0{0} {}
        Vector0(int n) :elem{new T[n]}, space{elem + n}, last{elem} {}
        // ...
    private:
        own<T*> elem;
        T* space;
        T* last;
    };
<!-- 
This is nice and general, but setting a `Vector0` to empty after an error involves an allocation, which may fail.
Also, having a default `Vector` represented as `{new T[0], 0, 0}` seems wasteful.
For example, `Vector0<int> v[100]` costs 100 allocations. -->

这个很不错且通用，但是，错误之后将`Vector0`设置为空包含到一个(new)分配，这可能会失败（译注：new可能会抛出异常，如new T[0]，如果抛出异常，成员变量都是未初始化的）。
此外, 将默认的 `Vector`设置为`{new T[0], 0, 0}`似乎是浪费的，
例如, `Vector0<int> v[100]`的成本为100次(new)分配。

##### Example

    template<typename T>
    // elem是nullptr或指向使用new分配的space-elem元素
    class Vector1 {
    public:
        // 设置为{nullptr, nullptr, nullptr}，且不会抛出异常
        Vector1() noexcept {}
        Vector1(int n) :elem{new T[n]}, space{elem + n}, last{elem} {}
        // ...
    private:
        own<T*> elem = nullptr;
        T* space = nullptr;
        T* last = nullptr;
    };

使用`{nullptr, nullptr, nullptr}`让`Vector1{}`很廉价，但特殊情况意味着需要运行时检查。
检测到错误之后将`Vector1`设置为空是平凡的(trivial)。

##### Enforcement

* 标出会抛出异常的默认构造函数。

### <a name="Rc-default"></a>C.45: 如果只是初始化数据成员，不要定义默认构造函数；使用类内成员初始化器来代替

##### Reason

使用类内成员初始化器可以让编译器为您生成(初始化)函数，编译器生成的函数可能会更有效。


##### Example, bad

    class X1 { // BAD: 没有使用成员初始化器
        string s;
        int i;
    public:
        X1() :s{"default"}, i{1} { }
        // ...
    };

##### Example

    class X2 {
        string s = "default";
        int i = 1;
    public:
        // use compiler-generated default constructor
        // ...
    };

##### Enforcement

(Simple) A default constructor should do more than just initialize member variables with constants.

### <a name="Rc-explicit"></a>C.46: By default, declare single-argument constructors explicit

##### Reason

To avoid unintended conversions.

##### Example, bad

    class String {
        // ...
    public:
        String(int);   // BAD
        // ...
    };

    String s = 10;   // surprise: string of size 10

##### Exception

If you really want an implicit conversion from the constructor argument type to the class type, don't use `explicit`:

    class Complex {
        // ...
    public:
        Complex(double d);   // OK: we want a conversion from d to {d, 0}
        // ...
    };

    Complex z = 10.7;   // unsurprising conversion

**See also**: [Discussion of implicit conversions](#Ro-conversion)

##### Note

Copy and move constructors should not be made `explicit` because they do not perform conversions. Explicit copy/move constructors make passing and returning by value difficult.

##### Enforcement

(Simple) Single-argument constructors should be declared `explicit`. Good single argument non-`explicit` constructors are rare in most code based. Warn for all that are not on a "positive list".

### <a name="Rc-order"></a>C.47: Define and initialize member variables in the order of member declaration

##### Reason

To minimize confusion and errors. That is the order in which the initialization happens (independent of the order of member initializers).

##### Example, bad

    class Foo {
        int m1;
        int m2;
    public:
        Foo(int x) :m2{x}, m1{++x} { }   // BAD: misleading initializer order
        // ...
    };

    Foo x(1); // surprise: x.m1 == x.m2 == 2

##### Enforcement

(Simple) A member initializer list should mention the members in the same order they are declared.

**See also**: [Discussion](#Sd-order)

### <a name="Rc-in-class-initializer"></a>C.48: Prefer in-class initializers to member initializers in constructors for constant initializers

##### Reason

Makes it explicit that the same value is expected to be used in all constructors. Avoids repetition. Avoids maintenance problems. It leads to the shortest and most efficient code.

##### Example, bad

    class X {   // BAD
        int i;
        string s;
        int j;
    public:
        X() :i{666}, s{"qqq"} { }   // j is uninitialized
        X(int ii) :i{ii} {}         // s is "" and j is uninitialized
        // ...
    };

How would a maintainer know whether `j` was deliberately uninitialized (probably a poor idea anyway) and whether it was intentional to give `s` the default value `""` in one case and `qqq` in another (almost certainly a bug)? The problem with `j` (forgetting to initialize a member) often happens when a new member is added to an existing class.

##### Example

    class X2 {
        int i {666};
        string s {"qqq"};
        int j {0};
    public:
        X2() = default;        // all members are initialized to their defaults
        X2(int ii) :i{ii} {}   // s and j initialized to their defaults
        // ...
    };

**Alternative**: We can get part of the benefits from default arguments to constructors, and that is not uncommon in older code. However, that is less explicit, causes more arguments to be passed, and is repetitive when there is more than one constructor:

    class X3 {   // BAD: inexplicit, argument passing overhead
        int i;
        string s;
        int j;
    public:
        X3(int ii = 666, const string& ss = "qqq", int jj = 0)
            :i{ii}, s{ss}, j{jj} { }   // all members are initialized to their defaults
        // ...
    };

##### Enforcement

* (Simple) Every constructor should initialize every member variable (either explicitly, via a delegating ctor call or via default construction).
* (Simple) Default arguments to constructors suggest an in-class initializer may be more appropriate.

### <a name="Rc-initialize"></a>C.49: Prefer initialization to assignment in constructors

##### Reason

An initialization explicitly states that initialization, rather than assignment, is done and can be more elegant and efficient. Prevents "use before set" errors.

##### Example, good

    class A {   // Good
        string s1;
    public:
        A(czstring p) : s1{p} { }    // GOOD: directly construct (and the C-string is explicitly named)
        // ...
    };

##### Example, bad

    class B {   // BAD
        string s1;
    public:
        B(const char* p) { s1 = p; }   // BAD: default constructor followed by assignment
        // ...
    };

    class C {   // UGLY, aka very bad
        int* p;
    public:
        C() { cout << *p; p = new int{10}; }   // accidental use before initialized
        // ...
    };

##### Example, better still

Instead of those `const char*`s we could use `gsl::string_span or (in C++17) `std::string_view`
as [a more general way to present arguments to a function](#Rstr-view):

    class D {   // Good
        string s1;
    public:
        A(string_view v) : s1{v} { }    // GOOD: directly construct
        // ...
    };

### <a name="Rc-factory"></a>C.50: Use a factory function if you need "virtual behavior" during initialization

##### Reason

If the state of a base class object must depend on the state of a derived part of the object, we need to use a virtual function (or equivalent) while minimizing the window of opportunity to misuse an imperfectly constructed object.

##### Note

The return type of the factory should normally be `unique_ptr` by default; if some uses are shared, the caller can `move` the `unique_ptr` into a `shared_ptr`. However, if the factory author knows that all uses of the returned object will be shared uses, return `shared_ptr` and use `make_shared` in the body to save an allocation.

##### Example, bad

    class B {
    public:
        B()
        {
            // ...
            f();   // BAD: virtual call in constructor
            // ...
        }

        virtual void f() = 0;

        // ...
    };

##### Example

    class B {
    protected:
        B() { /* ... */ }              // create an imperfectly initialized object

        virtual void PostInitialize()  // to be called right after construction
        {
            // ...
            f();    // GOOD: virtual dispatch is safe
            // ...
        }

    public:
        virtual void f() = 0;

        template<class T>
        static shared_ptr<T> Create()  // interface for creating shared objects
        {
            auto p = make_shared<T>();
            p->PostInitialize();
            return p;
        }
    };

    class D : public B { /* ... */ };  // some derived class

    shared_ptr<D> p = D::Create<D>();  // creating a D object

By making the constructor `protected` we avoid an incompletely constructed object escaping into the wild.
By providing the factory function `Create()`, we make construction (on the free store) convenient.

##### Note

Conventional factory functions allocate on the free store, rather than on the stack or in an enclosing object.

**See also**: [Discussion](#Sd-factory)

### <a name="Rc-delegating"></a>C.51: Use delegating constructors to represent common actions for all constructors of a class

##### Reason

To avoid repetition and accidental differences.

##### Example, bad

    class Date {   // BAD: repetitive
        int d;
        Month m;
        int y;
    public:
        Date(int dd, Month mm, year yy)
            :d{dd}, m{mm}, y{yy}
            { if (!valid(d, m, y)) throw Bad_date{}; }

        Date(int dd, Month mm)
            :d{dd}, m{mm} y{current_year()}
            { if (!valid(d, m, y)) throw Bad_date{}; }
        // ...
    };

The common action gets tedious to write and may accidentally not be common.

##### Example

    class Date2 {
        int d;
        Month m;
        int y;
    public:
        Date2(int dd, Month mm, year yy)
            :d{dd}, m{mm}, y{yy}
            { if (!valid(d, m, y)) throw Bad_date{}; }

        Date2(int dd, Month mm)
            :Date2{dd, mm, current_year()} {}
        // ...
    };

**See also**: If the "repeated action" is a simple initialization, consider [an in-class member initializer](#Rc-in-class-initializer).

##### Enforcement

(Moderate) Look for similar constructor bodies.

### <a name="Rc-inheriting"></a>C.52: Use inheriting constructors to import constructors into a derived class that does not need further explicit initialization

##### Reason

If you need those constructors for a derived class, re-implementing them is tedious and error-prone.

##### Example

`std::vector` has a lot of tricky constructors, so if I want my own `vector`, I don't want to reimplement them:

    class Rec {
        // ... data and lots of nice constructors ...
    };

    class Oper : public Rec {
        using Rec::Rec;
        // ... no data members ...
        // ... lots of nice utility functions ...
    };

##### Example, bad

    struct Rec2 : public Rec {
        int x;
        using Rec::Rec;
    };

    Rec2 r {"foo", 7};
    int val = r.x;   // uninitialized

##### Enforcement

Make sure that every member of the derived class is initialized.

## <a name="SS-copy"></a>C.copy: Copy and move

Value types should generally be copyable, but interfaces in a class hierarchy should not.
Resource handles may or may not be copyable.
Types can be defined to move for logical as well as performance reasons.

### <a name="Rc-copy-assignment"></a>C.60: Make copy assignment non-`virtual`, take the parameter by `const&`, and return by non-`const&`

##### Reason

It is simple and efficient. If you want to optimize for rvalues, provide an overload that takes a `&&` (see [F.18](#Rf-consume)).

##### Example

    class Foo {
    public:
        Foo& operator=(const Foo& x)
        {
            // GOOD: no need to check for self-assignment (other than performance)
            auto tmp = x;
            swap(tmp); // see C.83
            return *this;
        }
        // ...
    };

    Foo a;
    Foo b;
    Foo f();

    a = b;    // assign lvalue: copy
    a = f();  // assign rvalue: potentially move

##### Note

The `swap` implementation technique offers the [strong guarantee](#Abrahams01).

##### Example

But what if you can get significantly better performance by not making a temporary copy? Consider a simple `Vector` intended for a domain where assignment of large, equal-sized `Vector`s is common. In this case, the copy of elements implied by the `swap` implementation technique could cause an order of magnitude increase in cost:

    template<typename T>
    class Vector {
    public:
        Vector& operator=(const Vector&);
        // ...
    private:
        T* elem;
        int sz;
    };

    Vector& Vector::operator=(const Vector& a)
    {
        if (a.sz > sz) {
            // ... use the swap technique, it can't be bettered ...
            return *this
        }
        // ... copy sz elements from *a.elem to elem ...
        if (a.sz < sz) {
            // ... destroy the surplus elements in *this* and adjust size ...
        }
        return *this;
    }

By writing directly to the target elements, we will get only [the basic guarantee](#Abrahams01) rather than the strong guarantee offered by the `swap` technique. Beware of [self-assignment](#Rc-copy-self).

**Alternatives**: If you think you need a `virtual` assignment operator, and understand why that's deeply problematic, don't call it `operator=`. Make it a named function like `virtual void assign(const Foo&)`.
See [copy constructor vs. `clone()`](#Rc-copy-virtual).

##### Enforcement

* (Simple) An assignment operator should not be virtual. Here be dragons!
* (Simple) An assignment operator should return `T&` to enable chaining, not alternatives like `const T&` which interfere with composability and putting objects in containers.
* (Moderate) An assignment operator should (implicitly or explicitly) invoke all base and member assignment operators.
  Look at the destructor to determine if the type has pointer semantics or value semantics.

### <a name="Rc-copy-semantic"></a>C.61: A copy operation should copy

##### Reason

That is the generally assumed semantics. After `x = y`, we should have `x == y`.
After a copy `x` and `y` can be independent objects (value semantics, the way non-pointer built-in types and the standard-library types work) or refer to a shared object (pointer semantics, the way pointers work).

##### Example

    class X {   // OK: value semantics
    public:
        X();
        X(const X&);     // copy X
        void modify();   // change the value of X
        // ...
        ~X() { delete[] p; }
    private:
        T* p;
        int sz;
    };

    bool operator==(const X& a, const X& b)
    {
        return a.sz == b.sz && equal(a.p, a.p + a.sz, b.p, b.p + b.sz);
    }

    X::X(const X& a)
        :p{new T[a.sz]}, sz{a.sz}
    {
        copy(a.p, a.p + sz, p);
    }

    X x;
    X y = x;
    if (x != y) throw Bad{};
    x.modify();
    if (x == y) throw Bad{};   // assume value semantics

##### Example

    class X2 {  // OK: pointer semantics
    public:
        X2();
        X2(const X2&) = default; // shallow copy
        ~X2() = default;
        void modify();          // change the pointed-to value
        // ...
    private:
        T* p;
        int sz;
    };

    bool operator==(const X2& a, const X2& b)
    {
        return a.sz == b.sz && a.p == b.p;
    }

    X2 x;
    X2 y = x;
    if (x != y) throw Bad{};
    x.modify();
    if (x != y) throw Bad{};  // assume pointer semantics

##### Note

Prefer copy semantics unless you are building a "smart pointer". Value semantics is the simplest to reason about and what the standard-library facilities expect.

##### Enforcement

(Not enforceable)

### <a name="Rc-copy-self"></a>C.62: Make copy assignment safe for self-assignment

##### Reason

If `x = x` changes the value of `x`, people will be surprised and bad errors will occur (often including leaks).

##### Example

The standard-library containers handle self-assignment elegantly and efficiently:

    std::vector<int> v = {3, 1, 4, 1, 5, 9};
    v = v;
    // the value of v is still {3, 1, 4, 1, 5, 9}

##### Note

The default assignment generated from members that handle self-assignment correctly handles self-assignment.

    struct Bar {
        vector<pair<int, int>> v;
        map<string, int> m;
        string s;
    };

    Bar b;
    // ...
    b = b;   // correct and efficient

##### Note

You can handle self-assignment by explicitly testing for self-assignment, but often it is faster and more elegant to cope without such a test (e.g., [using `swap`](#Rc-swap)).

    class Foo {
        string s;
        int i;
    public:
        Foo& operator=(const Foo& a);
        // ...
    };

    Foo& Foo::operator=(const Foo& a)   // OK, but there is a cost
    {
        if (this == &a) return *this;
        s = a.s;
        i = a.i;
        return *this;
    }

This is obviously safe and apparently efficient.
However, what if we do one self-assignment per million assignments?
That's about a million redundant tests (but since the answer is essentially always the same, the computer's branch predictor will guess right essentially every time).
Consider:

    Foo& Foo::operator=(const Foo& a)   // simpler, and probably much better
    {
        s = a.s;
        i = a.i;
        return *this;
    }

`std::string` is safe for self-assignment and so are `int`. All the cost is carried by the (rare) case of self-assignment.

##### Enforcement

(Simple) Assignment operators should not contain the pattern `if (this == &a) return *this;` ???

### <a name="Rc-move-assignment"></a>C.63: Make move assignment non-`virtual`, take the parameter by `&&`, and return by non-`const &`

##### Reason

It is simple and efficient.

**See**: [The rule for copy-assignment](#Rc-copy-assignment).

##### Enforcement

Equivalent to what is done for [copy-assignment](#Rc-copy-assignment).

* (Simple) An assignment operator should not be virtual. Here be dragons!
* (Simple) An assignment operator should return `T&` to enable chaining, not alternatives like `const T&` which interfere with composability and putting objects in containers.
* (Moderate) A move assignment operator should (implicitly or explicitly) invoke all base and member move assignment operators.

### <a name="Rc-move-semantic"></a>C.64: A move operation should move and leave its source in a valid state

##### Reason

That is the generally assumed semantics.
After `y = std::move(x)` the value of `y` should be the value `x` had and `x` should be in a valid state.

##### Example

    template<typename T>
    class X {   // OK: value semantics
    public:
        X();
        X(X&& a) noexcept;  // move X
        void modify();     // change the value of X
        // ...
        ~X() { delete[] p; }
    private:
        T* p;
        int sz;
    };


    X::X(X&& a)
        :p{a.p}, sz{a.sz}  // steal representation
    {
        a.p = nullptr;     // set to "empty"
        a.sz = 0;
    }

    void use()
    {
        X x{};
        // ...
        X y = std::move(x);
        x = X{};   // OK
    } // OK: x can be destroyed

##### Note

Ideally, that moved-from should be the default value of the type.
Ensure that unless there is an exceptionally good reason not to.
However, not all types have a default value and for some types establishing the default value can be expensive.
The standard requires only that the moved-from object can be destroyed.
Often, we can easily and cheaply do better: The standard library assumes that it is possible to assign to a moved-from object.
Always leave the moved-from object in some (necessarily specified) valid state.

##### Note

Unless there is an exceptionally strong reason not to, make `x = std::move(y); y = z;` work with the conventional semantics.

##### Enforcement

(Not enforceable) Look for assignments to members in the move operation. If there is a default constructor, compare those assignments to the initializations in the default constructor.

### <a name="Rc-move-self"></a>C.65: Make move assignment safe for self-assignment

##### Reason

If `x = x` changes the value of `x`, people will be surprised and bad errors may occur. However, people don't usually directly write a self-assignment that turn into a move, but it can occur. However, `std::swap` is implemented using move operations so if you accidentally do `swap(a, b)` where `a` and `b` refer to the same object, failing to handle self-move could be a serious and subtle error.

##### Example

    class Foo {
        string s;
        int i;
    public:
        Foo& operator=(Foo&& a);
        // ...
    };

    Foo& Foo::operator=(Foo&& a) noexcept  // OK, but there is a cost
    {
        if (this == &a) return *this;  // this line is redundant
        s = std::move(a.s);
        i = a.i;
        return *this;
    }

The one-in-a-million argument against `if (this == &a) return *this;` tests from the discussion of [self-assignment](#Rc-copy-self) is even more relevant for self-move.

##### Note

There is no known general way of avoiding an `if (this == &a) return *this;` test for a move assignment and still get a correct answer (i.e., after `x = x` the value of `x` is unchanged).

##### Note

The ISO standard guarantees only a "valid but unspecified" state for the standard-library containers. Apparently this has not been a problem in about 10 years of experimental and production use. Please contact the editors if you find a counter example. The rule here is more caution and insists on complete safety.

##### Example

Here is a way to move a pointer without a test (imagine it as code in the implementation a move assignment):

    // move from other.ptr to this->ptr
    T* temp = other.ptr;
    other.ptr = nullptr;
    delete ptr;
    ptr = temp;

##### Enforcement

* (Moderate) In the case of self-assignment, a move assignment operator should not leave the object holding pointer members that have been `delete`d or set to `nullptr`.
* (Not enforceable) Look at the use of standard-library container types (incl. `string`) and consider them safe for ordinary (not life-critical) uses.

### <a name="Rc-move-noexcept"></a>C.66: Make move operations `noexcept`

##### Reason

A throwing move violates most people's reasonably assumptions.
A non-throwing move will be used more efficiently by standard-library and language facilities.

##### Example

    template<typename T>
    class Vector {
        // ...
        Vector(Vector&& a) noexcept :elem{a.elem}, sz{a.sz} { a.sz = 0; a.elem = nullptr; }
        Vector& operator=(Vector&& a) noexcept { elem = a.elem; sz = a.sz; a.sz = 0; a.elem = nullptr; }
        // ...
    public:
        T* elem;
        int sz;
    };

These operations do not throw.

##### Example, bad

    template<typename T>
    class Vector2 {
        // ...
        Vector2(Vector2&& a) { *this = a; }             // just use the copy
        Vector2& operator=(Vector2&& a) { *this = a; }  // just use the copy
        // ...
    public:
        T* elem;
        int sz;
    };

This `Vector2` is not just inefficient, but since a vector copy requires allocation, it can throw.

##### Enforcement

(Simple) A move operation should be marked `noexcept`.

### <a name="Rc-copy-virtual"></a>C.67: A polymorphic class should suppress copying

##### Reason

A *polymorphic class* is a class that defines or inherits at least one virtual function. It is likely that it will be used as a base class for other derived classes with polymorphic behavior. If it is accidentally passed by value, with the implicitly generated copy constructor and assignment, we risk slicing: only the base portion of a derived object will be copied, and the polymorphic behavior will be corrupted.

##### Example, bad

    class B { // BAD: polymorphic base class doesn't suppress copying
    public:
        virtual char m() { return 'B'; }
        // ... nothing about copy operations, so uses default ...
    };

    class D : public B {
    public:
        char m() override { return 'D'; }
        // ...
    };

    void f(B& b) {
        auto b2 = b; // oops, slices the object; b2.m() will return 'B'
    }

    D d;
    f(d);

##### Example

    class B { // GOOD: polymorphic class suppresses copying
    public:
        B(const B&) = delete;
        B& operator=(const B&) = delete;
        virtual char m() { return 'B'; }
        // ...
    };

    class D : public B {
    public:
        char m() override { return 'D'; }
        // ...
    };

    void f(B& b) {
        auto b2 = b; // ok, compiler will detect inadvertent copying, and protest
    }

    D d;
    f(d);

##### Note

If you need to create deep copies of polymorphic objects, use `clone()` functions: see [C.130](#Rh-copy).

##### Exception

Classes that represent exception objects need both to be polymorphic and copy-constructible.

##### Enforcement

* Flag a polymorphic class with a non-deleted copy operation.
* Flag an assignment of polymorphic class objects.

## C.other: Other default operation rules

In addition to the operations for which the language offer default implementations,
there are a few operations that are so foundational that it rules for their definition are needed:
comparisons, `swap`, and `hash`.

### <a name="Rc-eqdefault"></a>C.80: Use `=default` if you have to be explicit about using the default semantics

##### Reason

The compiler is more likely to get the default semantics right and you cannot implement these functions better than the compiler.

##### Example

    class Tracer {
        string message;
    public:
        Tracer(const string& m) : message{m} { cerr << "entering " << message << '\n'; }
        ~Tracer() { cerr << "exiting " << message << '\n'; }

        Tracer(const Tracer&) = default;
        Tracer& operator=(const Tracer&) = default;
        Tracer(Tracer&&) = default;
        Tracer& operator=(Tracer&&) = default;
    };

Because we defined the destructor, we must define the copy and move operations. The `= default` is the best and simplest way of doing that.

##### Example, bad

    class Tracer2 {
        string message;
    public:
        Tracer2(const string& m) : message{m} { cerr << "entering " << message << '\n'; }
        ~Tracer2() { cerr << "exiting " << message << '\n'; }

        Tracer2(const Tracer2& a) : message{a.message} {}
        Tracer2& operator=(const Tracer2& a) { message = a.message; return *this; }
        Tracer2(Tracer2&& a) :message{a.message} {}
        Tracer2& operator=(Tracer2&& a) { message = a.message; return *this; }
    };

Writing out the bodies of the copy and move operations is verbose, tedious, and error-prone. A compiler does it better.

##### Enforcement

(Moderate) The body of a special operation should not have the same accessibility and semantics as the compiler-generated version, because that would be redundant

### <a name="Rc-delete"></a>C.81: Use `=delete` when you want to disable default behavior (without wanting an alternative)

##### Reason

In a few cases, a default operation is not desirable.

##### Example

    class Immortal {
    public:
        ~Immortal() = delete;   // do not allow destruction
        // ...
    };

    void use()
    {
        Immortal ugh;   // error: ugh cannot be destroyed
        Immortal* p = new Immortal{};
        delete p;       // error: cannot destroy *p
    }

##### Example

A `unique_ptr` can be moved, but not copied. To achieve that its copy operations are deleted. To avoid copying it is necessary to `=delete` its copy operations from lvalues:

    template <class T, class D = default_delete<T>> class unique_ptr {
    public:
        // ...
        constexpr unique_ptr() noexcept;
        explicit unique_ptr(pointer p) noexcept;
        // ...
        unique_ptr(unique_ptr&& u) noexcept;   // move constructor
        // ...
        unique_ptr(const unique_ptr&) = delete; // disable copy from lvalue
        // ...
    };

    unique_ptr<int> make();   // make "something" and return it by moving

    void f()
    {
        unique_ptr<int> pi {};
        auto pi2 {pi};      // error: no move constructor from lvalue
        auto pi3 {make()};  // OK, move: the result of make() is an rvalue
    }

Note that deleted functions should be public.

##### Enforcement

The elimination of a default operation is (should be) based on the desired semantics of the class. Consider such classes suspect, but maintain a "positive list" of classes where a human has asserted that the semantics is correct.

### <a name="Rc-ctor-virtual"></a>C.82: Don't call virtual functions in constructors and destructors

##### Reason

The function called will be that of the object constructed so far, rather than a possibly overriding function in a derived class.
This can be most confusing.
Worse, a direct or indirect call to an unimplemented pure virtual function from a constructor or destructor results in undefined behavior.

##### Example, bad

    class Base {
    public:
        virtual void f() = 0;   // not implemented
        virtual void g();       // implemented with Base version
        virtual void h();       // implemented with Base version
    };

    class Derived : public Base {
    public:
        void g() override;   // provide Derived implementation
        void h() final;      // provide Derived implementation

        Derived()
        {
            // BAD: attempt to call an unimplemented virtual function
            f();

            // BAD: will call Derived::g, not dispatch further virtually
            g();

            // GOOD: explicitly state intent to call only the visible version
            Derived::g();

            // ok, no qualification needed, h is final
            h();
        }
    };

Note that calling a specific explicitly qualified function is not a virtual call even if the function is `virtual`.

**See also** [factory functions](#Rc-factory) for how to achieve the effect of a call to a derived class function without risking undefined behavior.

##### Note

There is nothing inherently wrong with calling virtual functions from constructors and destructors.
The semantics of such calls is type safe.
However, experience shows that such calls are rarely needed, easily confuse maintainers, and become a source of errors when used by novices.

##### Enforcement

* Flag calls of virtual functions from constructors and destructors.

### <a name="Rc-swap"></a>C.83: For value-like types, consider providing a `noexcept` swap function

##### Reason

A `swap` can be handy for implementing a number of idioms, from smoothly moving objects around to implementing assignment easily to providing a guaranteed commit function that enables strongly error-safe calling code. Consider using swap to implement copy assignment in terms of copy construction. See also [destructors, deallocation, and swap must never fail](#Re-never-fail).

##### Example, good

    class Foo {
        // ...
    public:
        void swap(Foo& rhs) noexcept
        {
            m1.swap(rhs.m1);
            std::swap(m2, rhs.m2);
        }
    private:
        Bar m1;
        int m2;
    };

Providing a nonmember `swap` function in the same namespace as your type for callers' convenience.

    void swap(Foo& a, Foo& b)
    {
        a.swap(b);
    }

##### Enforcement

* (Simple) A class without virtual functions should have a `swap` member function declared.
* (Simple) When a class has a `swap` member function, it should be declared `noexcept`.

### <a name="Rc-swap-fail"></a>C.84: A `swap` function may not fail

##### Reason

 `swap` is widely used in ways that are assumed never to fail and programs cannot easily be written to work correctly in the presence of a failing `swap`. The standard-library containers and algorithms will not work correctly if a swap of an element type fails.

##### Example, bad

    void swap(My_vector& x, My_vector& y)
    {
        auto tmp = x;   // copy elements
        x = y;
        y = tmp;
    }

This is not just slow, but if a memory allocation occurs for the elements in `tmp`, this `swap` may throw and would make STL algorithms fail if used with them.

##### Enforcement

(Simple) When a class has a `swap` member function, it should be declared `noexcept`.

### <a name="Rc-swap-noexcept"></a>C.85: Make `swap` `noexcept`

##### Reason

 [A `swap` may not fail](#Rc-swap-fail).
If a `swap` tries to exit with an exception, it's a bad design error and the program had better terminate.

##### Enforcement

(Simple) When a class has a `swap` member function, it should be declared `noexcept`.

### <a name="Rc-eq"></a>C.86: Make `==` symmetric with respect to operand types and `noexcept`

##### Reason

Asymmetric treatment of operands is surprising and a source of errors where conversions are possible.
`==` is a fundamental operations and programmers should be able to use it without fear of failure.

##### Example

    struct X {
        string name;
        int number;
    };

    bool operator==(const X& a, const X& b) noexcept {
        return a.name == b.name && a.number == b.number;
    }

##### Example, bad

    class B {
        string name;
        int number;
        bool operator==(const B& a) const {
            return name == a.name && number == a.number;
        }
        // ...
    };

`B`'s comparison accepts conversions for its second operand, but not its first.

##### Note

If a class has a failure state, like `double`'s `NaN`, there is a temptation to make a comparison against the failure state throw.
The alternative is to make two failure states compare equal and any valid state compare false against the failure state.

##### Note

This rule applies to all the usual comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.

##### Enforcement

* Flag an `operator==()` for which the argument types differ; same for other comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.
* Flag member `operator==()`s; same for other comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.

### <a name="Rc-eq-base"></a>C.87: Beware of `==` on base classes

##### Reason

It is really hard to write a foolproof and useful `==` for a hierarchy.

##### Example, bad

    class B {
        string name;
        int number;
        virtual bool operator==(const B& a) const
        {
             return name == a.name && number == a.number;
        }
        // ...
    };

`B`'s comparison accepts conversions for its second operand, but not its first.

    class D :B {
        char character;
        virtual bool operator==(const D& a) const
        {
            return name == a.name && number == a.number && character == a.character;
        }
        // ...
    };

    B b = ...
    D d = ...
    b == d;    // compares name and number, ignores d's character
    d == b;    // error: no == defined
    D d2;
    d == d2;   // compares name, number, and character
    B& b2 = d2;
    b2 == d;   // compares name and number, ignores d2's and d's character

Of course there are ways of making `==` work in a hierarchy, but the naive approaches do not scale

##### Note

This rule applies to all the usual comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.

##### Enforcement

* Flag a virtual `operator==()`; same for other comparison operators: `!=`, `<`, `<=`, `>`, and `>=`.

### <a name="Rc-hash"></a>C.89: Make a `hash` `noexcept`

##### Reason

Users of hashed containers use hash indirectly and don't expect simple access to throw.
It's a standard-library requirement.

##### Example, bad

    template<>
    struct hash<My_type> {  // thoroughly bad hash specialization
        using result_type = size_t;
        using argument_type = My_type;

        size_t operator() (const My_type & x) const
        {
            size_t xs = x.s.size();
            if (xs < 4) throw Bad_My_type{};    // "Nobody expects the Spanish inquisition!"
            return hash<size_t>()(x.s.size()) ^ trim(x.s);
        }
    };

    int main()
    {
        unordered_map<My_type, int> m;
        My_type mt{ "asdfg" };
        m[mt] = 7;
        cout << m[My_type{ "asdfg" }] << '\n';
    }

If you have to define a `hash` specialization, try simply to let it combine standard-library `hash` specializations with `^` (xor).
That tends to work better than "cleverness" for non-specialists.

##### Enforcement

* Flag throwing `hash`es.

## <a name="SS-containers"></a>C.con: Containers and other resource handles

A container is an object holding a sequence of objects of some type; `std::vector` is the archetypical container.
A resource handle is a class that owns a resource; `std::vector` is the typical resource handle; its resource is its sequence of elements.

Summary of container rules:

* [C.100: Follow the STL when defining a container](#Rcon-stl)
* [C.101: Give a container value semantics](#Rcon-val)
* [C.102: Give a container move operations](#Rcon-move)
* [C.103: Give a container an initializer list constructor](#Rcon-init)
* [C.104: Give a container a default constructor that sets it to empty](#Rcon-empty)
* ???
* [C.109: If a resource handle has pointer semantics, provide `*` and `->`](#Rcon-ptr)

**See also**: [Resources](#S-resource)


### <a name="Rcon-stl"></a>C.100: Follow the STL when defining a container

##### Reason

The STL containers are familiar to most C++ programmers and a fundamentally sound design.

##### Note

There are of course other fundamentally sound design styles and sometimes reasons to depart from
the style of the standard library, but in the absence of a solid reason to differ, it is simpler
and easier for both implementers and users to follow the standard.

In particular, `std::vector` and `std::map` provide useful relatively simple models.

##### Example

    // simplified (e.g., no allocators):

    template<typename T>
    class Sorted_vector {
        using value_type = T;
        // ... iterator types ...

        Sorted_vector() = default;
        Sorted_vector(initializer_list<T>);    // initializer-list constructor: sort and store
        Sorted_vector(const Sorted_vector&) = default;
        Sorted_vector(Sorted_vector&&) = default;
        Sorted_vector& operator=(const Sorted_vector&) = default;   // copy assignment
        Sorted_vector& operator=(Sorted_vector&&) = default;        // move assignment
        ~Sorted_vector() = default;

        Sorted_vector(const std::vector<T>& v);   // store and sort
        Sorted_vector(std::vector<T>&& v);        // sort and "steal representation"

        const T& operator[](int i) const { return rep[i]; }
        // no non-const direct access to preserve order

        void push_back(const T&);   // insert in the right place (not necessarily at back)
        void push_back(T&&);        // insert in the right place (not necessarily at back)

        // ... cbegin(), cend() ...
    private:
        std::vector<T> rep;  // use a std::vector to hold elements
    };

    template<typename T> bool operator==(const Sorted_vector<T>&, const Sorted_vector<T>&);
    template<typename T> bool operator!=(const Sorted_vector<T>&, const Sorted_vector<T>&);
    // ...

Here, the STL style is followed, but incompletely.
That's not uncommon.
Provide only as much functionality as makes sense for a specific container.
The key is to define the conventional constructors, assignments, destructors, and iterators
(as meaningful for the specific container) with their conventional semantics.
From that base, the container can be expanded as needed.
Here, special constructors from `std::vector` were added.

##### Enforcement

???

### <a name="Rcon-val"></a>C.101: Give a container value semantics

##### Reason

Regular objects are simpler to think and reason about than irregular ones.
Familiarity.

##### Note

If meaningful, make a container `Regular` (the concept).
In particular, ensure that an object compares equal to its copy.

##### Example

    void f(const Sorted_vector<string>& v)
    {
        Sorted_vector<string> v2 {v};
        if (v != v2)
            cout << "insanity rules!\n";
        // ...
    }

##### Enforcement

???

### <a name="Rcon-move"></a>C.102: Give a container move operations

##### Reason

Containers tend to get large; without a move constructor and a copy constructor an object can be
expensive to move around, thus tempting people to pass pointers to it around and getting into
resource management problems.

##### Example

    Sorted_vector<int> read_sorted(istream& is)
    {
        vector<int> v;
        cin >> v;   // assume we have a read operation for vectors
        Sorted_vector<int> sv = v;  // sorts
        return sv;
    }

    A user can reasonably assume that returning a standard-like container is cheap.

##### Enforcement

???

### <a name="Rcon-init"></a>C.103: Give a container an initializer list constructor

##### Reason

People expect to be able to initialize a container with a set of values.
Familiarity.

##### Example

    Sorted_vector<int> sv {1, 3, -1, 7, 0, 0}; // Sorted_vector sorts elements as needed

##### Enforcement

???

### <a name="Rcon-empty"></a>C.104: Give a container a default constructor that sets it to empty

##### Reason

To make it `Regular`.

##### Example

    vector<Sorted_sequence<string>> vs(100);    // 100 Sorted_sequences each with the value ""

##### Enforcement

???

### <a name="Rcon-ptr"></a>C.109: If a resource handle has pointer semantics, provide `*` and `->`

##### Reason

That's what is expected from pointers.
Familiarity.

##### Example

    ???

##### Enforcement

???

## <a name="SS-lambdas"></a>C.lambdas: Function objects and lambdas

A function object is an object supplying an overloaded `()` so that you can call it.
A lambda expression (colloquially often shortened to "a lambda") is a notation for generating a function object.
Function objects should be cheap to copy (and therefore [passed by value](#Rf-in)).

Summary:

* [F.50: Use a lambda when a function won't do (to capture local variables, or to write a local function)](#Rf-capture-vs-overload)
* [F.52: Prefer capturing by reference in lambdas that will be used locally, including passed to algorithms](#Rf-reference-capture)
* [F.53: Avoid capturing by reference in lambdas that will be used nonlocally, including returned, stored on the heap, or passed to another thread](#Rf-value-capture)
* [ES.28: Use lambdas for complex initialization, especially of `const` variables](#Res-lambda-init)

## <a name="SS-hier"></a>C.hier: Class hierarchies (OOP)

A class hierarchy is constructed to represent a set of hierarchically organized concepts (only).
Typically base classes act as interfaces.
There are two major uses for hierarchies, often named implementation inheritance and interface inheritance.

Class hierarchy rule summary:

* [C.120: Use class hierarchies to represent concepts with inherent hierarchical structure (only)](#Rh-domain)
* [C.121: If a base class is used as an interface, make it a pure abstract class](#Rh-abstract)
* [C.122: Use abstract classes as interfaces when complete separation of interface and implementation is needed](#Rh-separation)

Designing rules for classes in a hierarchy summary:

* [C.126: An abstract class typically doesn't need a constructor](#Rh-abstract-ctor)
* [C.127: A class with a virtual function should have a virtual or protected destructor](#Rh-dtor)
* [C.128: Virtual functions should specify exactly one of `virtual`, `override`, or `final`](#Rh-override)
* [C.129: When designing a class hierarchy, distinguish between implementation inheritance and interface inheritance](#Rh-kind)
* [C.130: For making deep copies of polymorphic classes prefer a virtual `clone` function instead of copy construction/assignment](#Rh-copy)
* [C.131: Avoid trivial getters and setters](#Rh-get)
* [C.132: Don't make a function `virtual` without reason](#Rh-virtual)
* [C.133: 避免使用受保护的数据](#Rh-protected)
* [C.134: Ensure all non-`const` data members have the same access level](#Rh-public)
* [C.135: Use multiple inheritance to represent multiple distinct interfaces](#Rh-mi-interface)
* [C.136: Use multiple inheritance to represent the union of implementation attributes](#Rh-mi-implementation)
* [C.137: Use `virtual` bases to avoid overly general base classes](#Rh-vbase)
* [C.138: Create an overload set for a derived class and its bases with `using`](#Rh-using)
* [C.139: Use `final` sparingly](#Rh-final)
* [C.140: Do not provide different default arguments for a virtual function and an overrider](#Rh-virtual-default-arg)

Accessing objects in a hierarchy rule summary:

* [C.145: Access polymorphic objects through pointers and references](#Rh-poly)
* [C.146: Use `dynamic_cast` where class hierarchy navigation is unavoidable](#Rh-dynamic_cast)
* [C.147: Use `dynamic_cast` to a reference type when failure to find the required class is considered an error](#Rh-ref-cast)
* [C.148: Use `dynamic_cast` to a pointer type when failure to find the required class is considered a valid alternative](#Rh-ptr-cast)
* [C.149: Use `unique_ptr` or `shared_ptr` to avoid forgetting to `delete` objects created using `new`](#Rh-smart)
* [C.150: Use `make_unique()` to construct objects owned by `unique_ptr`s](#Rh-make_unique)
* [C.151: Use `make_shared()` to construct objects owned by `shared_ptr`s](#Rh-make_shared)
* [C.152: Never assign a pointer to an array of derived class objects to a pointer to its base](#Rh-array)
* [C.153: Prefer virtual function to casting](#Rh-use-virtual)

### <a name="Rh-domain"></a>C.120: Use class hierarchies to represent concepts with inherent hierarchical structure (only)

##### Reason

Direct representation of ideas in code eases comprehension and maintenance. Make sure the idea represented in the base class exactly matches all derived types and there is not a better way to express it than using the tight coupling of inheritance.

Do *not* use inheritance when simply having a data member will do. Usually this means that the derived type needs to override a base virtual function or needs access to a protected member.

##### Example

    class DrawableUIElement {
    public:
        virtual void render() const = 0;
        // ...
    };

    class AbstractButton : public DrawableUIElement {
    public:
        virtual void onClick() = 0;
        // ...
    };

    class PushButton : public AbstractButton {
        virtual void render() const override;
        virtual void onClick() override;
        // ...
    };

    class Checkbox : public AbstractButton {
    // ...
    };

##### Example, bad

Do *not* represent non-hierarchical domain concepts as class hierarchies.

    template<typename T>
    class Container {
    public:
        // list operations:
        virtual T& get() = 0;
        virtual void put(T&) = 0;
        virtual void insert(Position) = 0;
        // ...
        // vector operations:
        virtual T& operator[](int) = 0;
        virtual void sort() = 0;
        // ...
        // tree operations:
        virtual void balance() = 0;
        // ...
    };

Here most overriding classes cannot implement most of the functions required in the interface well.
Thus the base class becomes an implementation burden.
Furthermore, the user of `Container` cannot rely on the member functions actually performing meaningful operations reasonably efficiently;
it may throw an exception instead.
Thus users have to resort to run-time checking and/or
not using this (over)general interface in favor of a particular interface found by a run-time type inquiry (e.g., a `dynamic_cast`).

##### Enforcement

* Look for classes with lots of members that do nothing but throw.
* Flag every use of a nonpublic base class `B` where the derived class `D` does not override a virtual function or access a protected member in `B`, and `B` is not one of the following: empty, a template parameter or parameter pack of `D`, a class template specialized with `D`.

### <a name="Rh-abstract"></a>C.121: If a base class is used as an interface, make it a pure abstract class

##### Reason

A class is more stable (less brittle) if it does not contain data.
Interfaces should normally be composed entirely of public pure virtual functions and a default/empty virtual destructor.

##### Example

    class My_interface {
    public:
        // ...only pure virtual functions here ...
        virtual ~My_interface() {}   // or =default
    };

##### Example, bad

    class Goof {
    public:
        // ...only pure virtual functions here ...
        // no virtual destructor
    };

    class Derived : public Goof {
        string s;
        // ...
    };

    void use()
    {
        unique_ptr<Goof> p {new Derived{"here we go"}};
        f(p.get()); // use Derived through the Goof interface
        g(p.get()); // use Derived through the Goof interface
    } // leak

The `Derived` is `delete`d through its `Goof` interface, so its `string` is leaked.
Give `Goof` a virtual destructor and all is well.


##### Enforcement

* Warn on any class that contains data members and also has an overridable (non-`final`) virtual function.

### <a name="Rh-separation"></a>C.122: Use abstract classes as interfaces when complete separation of interface and implementation is needed

##### Reason

Such as on an ABI (link) boundary.

##### Example

    struct Device {
        virtual ~Device() = default;
        virtual void write(span<const char> outbuf) = 0;
        virtual void read(span<char> inbuf) = 0;
    };

    class D1 : public Device {
        // ... data ...

        void write(span<const char> outbuf) override;
        void read(span<char> inbuf) override;
    };

    class D2 : public Device {
        // ... different data ...

        void write(span<const char> outbuf) override;
        void read(span<char> inbuf) override;
    };

A user can now use `D1`s and `D2`s interchangeably through the interface provided by `Device`.
Furthermore, we can update `D1` and `D2` in ways that are not binary compatible with older versions as long as all access goes through `Device`.

##### Enforcement

    ???

## C.hierclass: Designing classes in a hierarchy:

### <a name="Rh-abstract-ctor"></a>C.126: An abstract class typically doesn't need a constructor

##### Reason

An abstract class typically does not have any data for a constructor to initialize.

##### Example

    ???

##### Exception

* A base class constructor that does work, such as registering an object somewhere, may need a constructor.
* In extremely rare cases, you might find it reasonable for an abstract class to have a bit of data shared by all derived classes
  (e.g., use statistics data, debug information, etc.); such classes tend to have constructors. But be warned: Such classes also tend to be prone to requiring virtual inheritance.

##### Enforcement

Flag abstract classes with constructors.

### <a name="Rh-dtor"></a>C.127: A class with a virtual function should have a virtual or protected destructor

##### Reason

A class with a virtual function is usually (and in general) used via a pointer to base. Usually, the last user has to call delete on a pointer to base, often via a smart pointer to base, so the destructor should be public and virtual. Less commonly, if deletion through a pointer to base is not intended to be supported, the destructor should be protected and nonvirtual; see [C.35](#Rc-dtor-virtual).

##### Example, bad

    struct B {
        virtual int f() = 0;
        // ... no user-written destructor, defaults to public nonvirtual ...
    };

    // bad: derived from a class without a virtual destructor
    struct D : B {
        string s {"default"};
    };

    void use()
    {
        unique_ptr<B> p = make_unique<D>();
        // ...
    } // undefined behavior. May call B::~B only and leak the string

##### Note

There are people who don't follow this rule because they plan to use a class only through a `shared_ptr`: `std::shared_ptr<B> p = std::make_shared<D>(args);` Here, the shared pointer will take care of deletion, so no leak will occur from an inappropriate `delete` of the base. People who do this consistently can get a false positive, but the rule is important -- what if one was allocated using `make_unique`? It's not safe unless the author of `B` ensures that it can never be misused, such as by making all constructors private and providing a factory function to enforce the allocation with `make_shared`.

##### Enforcement

* A class with any virtual functions should have a destructor that is either public and virtual or else protected and nonvirtual.
* Flag `delete` of a class with a virtual function but no virtual destructor.

### <a name="Rh-override"></a>C.128: Virtual functions should specify exactly one of `virtual`, `override`, or `final`

##### Reason

Readability.
Detection of mistakes.
Writing explicit `virtual`, `override`, or `final` is self-documenting and enables the compiler to catch mismatch of types and/or names between base and derived classes. However, writing more than one of these three is both redundant and a potential source of errors.

It's simple and clear:

* `virtual` means exactly and only "this is a new virtual function."
* `override` means exactly and only "this is a non-final overrider."
* `final` means exactly and only "this is a final overrider."

If a base class destructor is declared `virtual`, one should avoid declaring derived class destructors  `virtual` or `override`. Some code base and tools might insist on `override` for destructors, but that is not the recommendation of these guidelines.

##### Example, bad

    struct B {
        void f1(int);
        virtual void f2(int) const;
        virtual void f3(int);
        // ...
    };

    struct D : B {
        void f1(int);        // bad (hope for a warning): D::f1() hides B::f1()
        void f2(int) const;  // bad (but conventional and valid): no explicit override
        void f3(double);     // bad (hope for a warning): D::f3() hides B::f3()
        // ...
    };

##### Example, good

    struct Better : B {
        void f1(int) override;        // error (caught): D::f1() hides B::f1()
        void f2(int) const override;
        void f3(double) override;     // error (caught): D::f3() hides B::f3()
        // ...
    };

#### Discussion

We want to eliminate two particular classes of errors:

* **implicit virtual**: the programmer intended the function to be implicitly virtual and it is (but readers of the code can't tell); or the programmer intended the function to be implicitly virtual but it isn't (e.g., because of a subtle parameter list mismatch); or the programmer did not intend the function to be virtual but it is (because it happens to have the same signature as a virtual in the base class)
* **implicit override**: the programmer intended the function to be implicitly an overrider and it is (but readers of the code can't tell); or the programmer intended the function to be implicitly an overrider but it isn't (e.g., because of a subtle parameter list mismatch); or the programmer did not intend the function to be an overrider but it is (because it happens to have the same signature as a virtual in the base class -- note this problem arises whether or not the function is explicitly declared virtual, because the programmer may have intended to create either a new virtual function or a new nonvirtual function)

##### Enforcement

* Compare virtual function names in base and derived classes and flag uses of the same name that does not override.
* Flag overrides with neither `override` nor `final`.
* Flag function declarations that use more than one of `virtual`, `override`, and `final`.

### <a name="Rh-kind"></a>C.129: When designing a class hierarchy, distinguish between implementation inheritance and interface inheritance

##### Reason

Implementation details in an interface make the interface brittle;
that is, make its users vulnerable to having to recompile after changes in the implementation.
Data in a base class increases the complexity of implementing the base and can lead to replication of code.

##### Note

Definition:

* interface inheritance is the use of inheritance to separate users from implementations,
in particular to allow derived classes to be added and changed without affecting the users of base classes.
* implementation inheritance is the use of inheritance to simplify implementation of new facilities
by making useful operations available for implementers of related new operations (sometimes called "programming by difference").

A pure interface class is simply a set of pure virtual functions; see [I.25](#Ri-abstract).

In early OOP (e.g., in the 1980s and 1990s), implementation inheritance and interface inheritance were often mixed
and bad habits die hard.
Even now, mixtures are not uncommon in old code bases and in old-style teaching material.

The importance of keeping the two kinds of inheritance increases

* with the size of a hierarchy (e.g., dozens of derived classes),
* with the length of time the hierarchy is used (e.g., decades), and
* with the number of distinct organizations in which a hierarchy is used
(e.g., it can be difficult to distribute an update to a base class)


##### Example, bad

    class Shape {   // BAD, mixed interface and implementation
    public:
        Shape();
        Shape(Point ce = {0, 0}, Color co = none): cent{ce}, col {co} { /* ... */}

        Point center() const { return cent; }
        Color color() const { return col; }

        virtual void rotate(int) = 0;
        virtual void move(Point p) { cent = p; redraw(); }

        virtual void redraw();

        // ...
    private:
        Point cent;
        Color col;
    };

    class Circle : public Shape {
    public:
        Circle(Point c, int r) :Shape{c}, rad{r} { /* ... */ }

        // ...
    private:
        int rad;
    };

    class Triangle : public Shape {
    public:
        Triangle(Point p1, Point p2, Point p3); // calculate center
        // ...
    };

Problems:

* As the hierarchy grows and more data is added to `Shape`, the constructors gets harder to write and maintain.
* Why calculate the center for the `Triangle`? we may never us it.
* Add a data member to `Shape` (e.g., drawing style or canvas)
and all derived classes and all users needs to be reviewed, possibly changes, and probably recompiled.

The implementation of `Shape::move()` is an example of implementation inheritance:
we have defined `move()` once and for all for all derived classes.
The more code there is in such base class member function implementations and the more data is shared by placing it in the base,
the more benefits we gain - and the less stable the hierarchy is.

##### Example

This Shape hierarchy can be rewritten using interface inheritance:

    class Shape {  // pure interface
    public:
        virtual Point center() const = 0;
        virtual Color color() const = 0;

        virtual void rotate(int) = 0;
        virtual void move(Point p) = 0;

        virtual void redraw() = 0;

        // ...
    };

Note that a pure interface rarely have constructors: there is nothing to construct.

    class Circle : public Shape {
    public:
        Circle(Point c, int r, Color c) :cent{c}, rad{r}, col{c} { /* ... */ }

        Point center() const override { return cent; }
        Color color() const override { return col; }

        // ...
    private:
        Point cent;
        int rad;
        Color col;
    };

The interface is now less brittle, but there is more work in implementing the member functions.
For example, `center` has to be implemented by every class derived from `Shape`.

##### Example, dual hierarchy

How can we gain the benefit of the stable hierarchies from implementation hierarchies and the benefit of implementation reuse from implementation inheritance.
One popular technique is dual hierarchies.
There are many ways of implementing the idea of dual hierarchies; here, we use a multiple-inheritance variant.

First we devise a hierarchy of interface classes:

    class Shape {   // pure interface
    public:
        virtual Point center() const = 0;
        virtual Color color() const = 0;

        virtual void rotate(int) = 0;
        virtual void move(Point p) = 0;

        virtual void redraw() = 0;

        // ...
    };

    class Circle : public virtual Shape {   // pure interface
    public:
        virtual int radius() = 0;
        // ...
    };

To make this interface useful, we must provide its implementation classes (here, named equivalently, but in the `Impl` namespace):

    class Impl::Shape : public virtual ::Shape { // implementation
    public:
        // constructors, destructor
        // ...
        Point center() const override { /* ... */ }
        Color color() const override { /* ... */ }

        void rotate(int) override { /* ... */ }
        void move(Point p) override { /* ... */ }

        void redraw() override { /* ... */ }

        // ...
    };

Now `Shape` is a poor example of a class with an implementation,
but bear with us because this is just a simple example of a technique aimed at more complex hierarchies.

    class Impl::Circle : public virtual ::Circle, public Impl::Shape {   // implementation
    public:
        // constructors, destructor

        int radius() override { /* ... */ }
        // ...
    };

And we could extend the hierarchies by adding a Smiley class (:-)):

    class Smiley : public virtual Circle { // pure interface
    public:
        // ...
    };

    class Impl::Smiley : public virtual ::Smiley, public Impl::Circle {   // implementation
    public:
        // constructors, destructor
        // ...
    }

There are now two hierarchies:

* interface: Smiley -> Circle -> Shape
* implementation: Impl::Smiley -> Impl::Circle -> Impl::Shape

Since each implementation is derived from its interface as well as its implementation base class we get a lattice (DAG):

    Smiley     ->         Circle     ->  Shape
      ^                     ^               ^
      |                     |               |
    Impl::Smiley -> Impl::Circle -> Impl::Shape

As mentioned, this is just one way to construct a dual hierarchy.

The implementation hierarchy can be used directly, rather than through the abstract interface.

    void work_with_shape(Shape&);

    int user()
    {
        Impl::Smiley my_smiley{ /* args */ };   // create concrete shape
        // ...
        my_smiley.some_member();        // use implementation class directly
        // ...
        work_with_shape(my_smiley);     // use implementation through abstract interface
        // ...
    }

This can be useful when the implementation class has members that are not offered in the abstract interface
or if direct use of a member offers optimization opportunities (e.g., if an implementation member function is `final`)

##### Note

Another (related) technique for separating interface and implementation is [Pimpl](#Ri-pimpl).

##### Note

There is often a choice between offering common functionality as (implemented) base class functions and free-standing functions
(in an implementation namespace).
Base classes gives a shorter notation and easier access to shared data (in the base)
at the cost of the functionality being available only to users of the hierarchy.

##### Enforcement

* Flag a derived to base conversion to a base with both data and virtual functions
(except for calls from a derived class member to a base class member)
* ???


### <a name="Rh-copy"></a>C.130: For making deep copies of polymorphic classes prefer a virtual `clone` function instead of copy construction/assignment

##### Reason

Copying a polymorphic class is discouraged due to the slicing problem, see [C.67](#Rc-copy-virtual). If you really need copy semantics, copy deeply: Provide a virtual `clone` function that will copy the actual most-derived type and return an owning pointer to the new object, and then in derived classes return the derived type (use a covariant return type).

##### Example

    class B {
    public:
        virtual owner<B*> clone() = 0;
        virtual ~B() = 0;

        B(const B&) = delete;
        B& operator=(const B&) = delete;
    };

    class D : public B {
    public:
        owner<D*> clone() override;
        virtual ~D() override;
    };

Generally, it is recommended to use smart pointers to represent ownership (see [R.20](#Rr-owner)). However, because of language rules, the covariant return type cannot be a smart pointer: `D::clone` can't return a `unique_ptr<D>` while `B::clone` returns `unique_ptr<B>`. Therefore, you either need to consistently return `unique_ptr<B>` in all overrides, or use `owner<>` utility from the [Guidelines Support Library](#SS-views).



### <a name="Rh-get"></a>C.131: Avoid trivial getters and setters

##### Reason

A trivial getter or setter adds no semantic value; the data item could just as well be `public`.

##### Example

    class Point {   // Bad: verbose
        int x;
        int y;
    public:
        Point(int xx, int yy) : x{xx}, y{yy} { }
        int get_x() const { return x; }
        void set_x(int xx) { x = xx; }
        int get_y() const { return y; }
        void set_y(int yy) { y = yy; }
        // no behavioral member functions
    };

Consider making such a class a `struct` -- that is, a behaviorless bunch of variables, all public data and no member functions.

    struct Point {
        int x {0};
        int y {0};
    };

Note that we can put default initializers on member variables: [C.49: Prefer initialization to assignment in constructors](#Rc-initialize).

##### Note

The key to this rule is whether the semantics of the getter/setter are trivial. While it is not a complete definition of "trivial", consider whether there would be any difference beyond syntax if the getter/setter was a public data member instead. Examples of non-trivial semantics would be: maintaining a class invariant or converting between an internal type and an interface type.

##### Enforcement

Flag multiple `get` and `set` member functions that simply access a member without additional semantics.

### <a name="Rh-virtual"></a>C.132: Don't make a function `virtual` without reason

##### Reason

Redundant `virtual` increases run-time and object-code size.
A virtual function can be overridden and is thus open to mistakes in a derived class.
A virtual function ensures code replication in a templated hierarchy.

##### Example, bad

    template<class T>
    class Vector {
    public:
        // ...
        virtual int size() const { return sz; }   // bad: what good could a derived class do?
    private:
        T* elem;   // the elements
        int sz;    // number of elements
    };

This kind of "vector" isn't meant to be used as a base class at all.

##### Enforcement

* Flag a class with virtual functions but no derived classes.
* Flag a class where all member functions are virtual and have implementations.

### <a name="Rh-protected"></a>C.133: 避免使用受保护的数据

##### Reason

`protected` data is a source of complexity and errors.
`protected` data complicates the statement of invariants.
`protected` data inherently violates the guidance against putting data in base classes, which usually leads to having to deal with virtual inheritance as well.

##### Example, bad

    class Shape {
    public:
        // ... interface functions ...
    protected:
        // data for use in derived classes:
        Color fill_color;
        Color edge_color;
        Style st;
    };

Now it is up to every derived `Shape` to manipulate the protected data correctly.
This has been popular, but also a major source of maintenance problems.
In a large class hierarchy, the consistent use of protected data is hard to maintain because there can be a lot of code,
spread over a lot of classes.
The set of classes that can touch that data is open: anyone can derive a new class and start manipulating the protected data.
Often, it is not possible to examine the complete set of classes, so any change to the representation of the class becomes infeasible.
There is no enforced invariant for the protected data; it is much like a set of global variables.
The protected data has de facto become global to a large body of code.

##### Note

Protected data often looks tempting to enable arbitrary improvements through derivation.
Often, what you get is unprincipled changes and errors.
[Prefer `private` data](#Rc-private) with a well-specified and enforced invariant.
Alternative, and often better, [keep data out of any class used as an interface](#Rh-abstract).

##### Note

Protected member function can be just fine.

##### Enforcement

Flag classes with `protected` data.

### <a name="Rh-public"></a>C.134: Ensure all non-`const` data members have the same access level

##### Reason

Prevention of logical confusion leading to errors.
If the non-`const` data members don't have the same access level, the type is confused about what it's trying to do.
Is it a type that maintains an invariant or simply a collection of values?

##### Discussion

The core question is: What code is responsible for maintaining a meaningful/correct value for that variable?

There are exactly two kinds of data members:

* A: Ones that don't participate in the object's invariant. Any combination of values for these members is valid.
* B: Ones that do participate in the object's invariant. Not every combination of values is meaningful (else there'd be no invariant). Therefore all code that has write access to these variables must know about the invariant, know the semantics, and know (and actively implement and enforce) the rules for keeping the values correct.

Data members in category A should just be `public` (or, more rarely, `protected` if you only want derived classes to see them). They don't need encapsulation. All code in the system might as well see and manipulate them.

Data members in category B should be `private` or `const`. This is because encapsulation is important. To make them non-`private` and non-`const` would mean that the object can't control its own state: An unbounded amount of code beyond the class would need to know about the invariant and participate in maintaining it accurately -- if these data members were `public`, that would be all calling code that uses the object; if they were `protected`, it would be all the code in current and future derived classes. This leads to brittle and tightly coupled code that quickly becomes a nightmare to maintain. Any code that inadvertently sets the data members to an invalid or unexpected combination of values would corrupt the object and all subsequent uses of the object.

Most classes are either all A or all B:

* *All public*: If you're writing an aggregate bundle-of-variables without an invariant across those variables, then all the variables should be `public`.
  [By convention, declare such classes `struct` rather than `class`](#Rc-struct)
* *All private*: If you're writing a type that maintains an invariant, then all the non-`const` variables should be private -- it should be encapsulated.

##### Exception

Occasionally classes will mix A and B, usually for debug reasons. An encapsulated object may contain something like non-`const` debug instrumentation that isn't part of the invariant and so falls into category A -- it isn't really part of the object's value or meaningful observable state either. In that case, the A parts should be treated as A's (made `public`, or in rarer cases `protected` if they should be visible only to derived classes) and the B parts should still be treated like B's (`private` or `const`).

##### Enforcement

Flag any class that has non-`const` data members with different access levels.

### <a name="Rh-mi-interface"></a>C.135: Use multiple inheritance to represent multiple distinct interfaces

##### Reason

Not all classes will necessarily support all interfaces, and not all callers will necessarily want to deal with all operations.
Especially to break apart monolithic interfaces into "aspects" of behavior supported by a given derived class.

##### Example

    class iostream : public istream, public ostream {   // very simplified
        // ...
    };

`istream` provides the interface to input operations; `ostream` provides the interface to output operations.
`iostream` provides the union of the `istream` and `ostream` interfaces and the synchronization needed to allow both on a single stream.

##### Note

This is a very common use of inheritance because the need for multiple different interfaces to an implementation is common
and such interfaces are often not easily or naturally organized into a single-rooted hierarchy.

##### Note

Such interfaces are typically abstract classes.

##### Enforcement

???

### <a name="Rh-mi-implementation"></a>C.136: Use multiple inheritance to represent the union of implementation attributes

##### Reason

Some forms of mixins have state and often operations on that state.
If the operations are virtual the use of inheritance is necessary, if not using inheritance can avoid boilerplate and forwarding.

##### Example

    class iostream : public istream, public ostream {   // very simplified
        // ...
    };

`istream` provides the interface to input operations (and some data); `ostream` provides the interface to output operations (and some data).
`iostream` provides the union of the `istream` and `ostream` interfaces and the synchronization needed to allow both on a single stream.

##### Note

This a relatively rare use because implementation can often be organized into a single-rooted hierarchy.

##### Example

Sometimes, an "implementation attribute" is more like a "mixin" that determine the behavior of an implementation and inject
members to enable the implementation of the policies it requires.
For example, see `std::enable_shared_from_this`
or various bases from boost.intrusive (e.g. `list_base_hook` or `intrusive_ref_counter`).

##### Enforcement

???

### <a name="Rh-vbase"></a>C.137: Use `virtual` bases to avoid overly general base classes

##### Reason

 Allow separation of shared data and interface.
 To avoid all shared data to being put into an ultimate base class.

##### Example

    struct Interface {
        virtual void f();
        virtual int g();
        // ... no data here ...
    };

    class Utility {  // with data
        void utility1();
        virtual void utility2();    // customization point
    public:
        int x;
        int y;
    };

    class Derive1 : public Interface, virtual protected Utility {
        // override Interface functions
        // Maybe override Utility virtual functions
        // ...
    };

    class Derive2 : public Interface, virtual protected Utility {
        // override Interface functions
        // Maybe override Utility virtual functions
        // ...
    };

Factoring out `Utility` makes sense if many derived classes share significant "implementation details."


##### Note

Obviously, the example is too "theoretical", but it is hard to find a *small* realistic example.
`Interface` is the root of an [interface hierarchy](#Rh-abstract)
and `Utility` is the root of an [implementation hierarchy](#Rh-kind).
Here is [a slightly more realistic example](https://www.quora.com/What-are-the-uses-and-advantages-of-virtual-base-class-in-C%2B%2B/answer/Lance-Diduck) with an explanation.

##### Note

Often, linearization of a hierarchy is a better solution.

##### Enforcement

Flag mixed interface and implementation hierarchies.

### <a name="Rh-using"></a>C.138: Create an overload set for a derived class and its bases with `using`

##### Reason

Without a using declaration, member functions in the derived class hide the entire inherited overload sets.

##### Example, bad

    #include <iostream>
    class B {
    public:
        virtual int f(int i) { std::cout << "f(int): "; return i; }
        virtual double f(double d) { std::cout << "f(double): "; return d; }
    };
    class D: public B {
    public:
        int f(int i) override { std::cout << "f(int): "; return i + 1; }
    };
    int main()
    {
        D d;
        std::cout << d.f(2) << '\n';   // prints "f(int): 3"
        std::cout << d.f(2.3) << '\n'; // prints "f(int): 3"
    }

##### Example, good

    class D: public B {
    public:
        int f(int i) override { std::cout << "f(int): "; return i + 1; }
        using B::f; // exposes f(double)
    };

##### Note

This issue affects both virtual and nonvirtual member functions

For variadic bases, C++17 introduced a variadic form of the using-declaration,

    template <class... Ts>
    struct Overloader : Ts... {
        using Ts::operator()...; // exposes operator() from every base
    };

##### Enforcement

Diagnose name hiding

### <a name="Rh-final"></a>C.139: Use `final` sparingly

##### Reason

Capping a hierarchy with `final` is rarely needed for logical reasons and can be damaging to the extensibility of a hierarchy.

##### Example, bad

    class Widget { /* ... */ };

    // nobody will ever want to improve My_widget (or so you thought)
    class My_widget final : public Widget { /* ... */ };

    class My_improved_widget : public My_widget { /* ... */ };  // error: can't do that

##### Note

Not every class is meant to be a base class.
Most standard-library classes are examples of that (e.g., `std::vector` and `std::string` are not designed to be derived from).
This rule is about using `final` on classes with virtual functions meant to be interfaces for a class hierarchy.

##### Note

Capping an individual virtual function with `final` is error-prone as `final` can easily be overlooked when defining/overriding a set of functions.
Fortunately, the compiler catches such mistakes: You cannot re-declare/re-open a `final` member in a derived class.

##### Note

Claims of performance improvements from `final` should be substantiated.
Too often, such claims are based on conjecture or experience with other languages.

There are examples where `final` can be important for both logical and performance reasons.
One example is a performance-critical AST hierarchy in a compiler or language analysis tool.
New derived classes are not added every year and only by library implementers.
However, misuses are (or at least have been) far more common.

##### Enforcement

Flag uses of `final`.


### <a name="Rh-virtual-default-arg"></a>C.140: Do not provide different default arguments for a virtual function and an overrider

##### Reason

That can cause confusion: An overrider does not inherit default arguments.

##### Example, bad

    class Base {
    public:
        virtual int multiply(int value, int factor = 2) = 0;
    };

    class Derived : public Base {
    public:
        int multiply(int value, int factor = 10) override;
    };

    Derived d;
    Base& b = d;

    b.multiply(10);  // these two calls will call the same function but
    d.multiply(10);  // with different arguments and so different results

##### Enforcement

Flag default arguments on virtual functions if they differ between base and derived declarations.

## C.hier-access: Accessing objects in a hierarchy

### <a name="Rh-poly"></a>C.145: Access polymorphic objects through pointers and references

##### Reason

If you have a class with a virtual function, you don't (in general) know which class provided the function to be used.

##### Example

    struct B { int a; virtual int f(); };
    struct D : B { int b; int f() override; };

    void use(B b)
    {
        D d;
        B b2 = d;   // slice
        B b3 = b;
    }

    void use2()
    {
        D d;
        use(d);   // slice
    }

Both `d`s are sliced.

##### Exception

You can safely access a named polymorphic object in the scope of its definition, just don't slice it.

    void use3()
    {
        D d;
        d.f();   // OK
    }

##### Enforcement

Flag all slicing.

### <a name="Rh-dynamic_cast"></a>C.146: Use `dynamic_cast` where class hierarchy navigation is unavoidable

##### Reason

`dynamic_cast` is checked at run time.

##### Example

    struct B {   // an interface
        virtual void f();
        virtual void g();
    };

    struct D : B {   // a wider interface
        void f() override;
        virtual void h();
    };

    void user(B* pb)
    {
        if (D* pd = dynamic_cast<D*>(pb)) {
            // ... use D's interface ...
        }
        else {
            // ... make do with B's interface ...
        }
    }

Use of the other casts can violate type safety and cause the program to access a variable that is actually of type `X` to be accessed as if it were of an unrelated type `Z`:

    void user2(B* pb)   // bad
    {
        D* pd = static_cast<D*>(pb);    // I know that pb really points to a D; trust me
        // ... use D's interface ...
    }

    void user3(B* pb)    // unsafe
    {
        if (some_condition) {
            D* pd = static_cast<D*>(pb);   // I know that pb really points to a D; trust me
            // ... use D's interface ...
        }
        else {
            // ... make do with B's interface ...
        }
    }

    void f()
    {
        B b;
        user(&b);   // OK
        user2(&b);  // bad error
        user3(&b);  // OK *if* the programmer got the some_condition check right
    }

##### Note

Like other casts, `dynamic_cast` is overused.
[Prefer virtual functions to casting](#Rh-use-virtual).
Prefer [static polymorphism](#???) to hierarchy navigation where it is possible (no run-time resolution necessary)
and reasonably convenient.

##### Note

Some people use `dynamic_cast` where a `typeid` would have been more appropriate;
`dynamic_cast` is a general "is kind of" operation for discovering the best interface to an object,
whereas `typeid` is a "give me the exact type of this object" operation to discover the actual type of an object.
The latter is an inherently simpler operation that ought to be faster.
The latter (`typeid`) is easily hand-crafted if necessary (e.g., if working on a system where RTTI is -- for some reason -- prohibited),
the former (`dynamic_cast`) is far harder to implement correctly in general.

Consider:

    struct B {
        const char* name {"B"};
        // if pb1->id() == pb2->id() *pb1 is the same type as *pb2
        virtual const char* id() const { return name; }
        // ...
    };

    struct D : B {
        const char* name {"D"};
        const char* id() const override { return name; }
        // ...
    };

    void use()
    {
        B* pb1 = new B;
        B* pb2 = new D;

        cout << pb1->id(); // "B"
        cout << pb2->id(); // "D"


        if (pb1->id() == "D") {         // looks innocent
            D* pd = static_cast<D*>(pb1);
            // ...
        }
        // ...
    }

The result of `pb2->id() == "D"` is actually implementation defined.
We added it to warn of the dangers of home-brew RTTI.
This code may work as expected for years, just to fail on a new machine, new compiler, or a new linker that does not unify character literals.

If you implement your own RTTI, be careful.

##### Exception

If your implementation provided a really slow `dynamic_cast`, you may have to use a workaround.
However, all workarounds that cannot be statically resolved involve explicit casting (typically `static_cast`) and are error-prone.
You will basically be crafting your own special-purpose `dynamic_cast`.
So, first make sure that your `dynamic_cast` really is as slow as you think it is (there are a fair number of unsupported rumors about)
and that your use of `dynamic_cast` is really performance critical.

We are of the opinion that current implementations of `dynamic_cast` are unnecessarily slow.
For example, under suitable conditions, it is possible to perform a `dynamic_cast` in [fast constant time](http://www.stroustrup.com/fast_dynamic_casting.pdf).
However, compatibility makes changes difficult even if all agree that an effort to optimize is worthwhile.

In very rare cases, if you have measured that the `dynamic_cast` overhead is material, you have other means to statically guarantee that a downcast will succeed (e.g., you are using CRTP carefully), and there is no virtual inheritance involved, consider tactically resorting `static_cast` with a prominent comment and disclaimer summarizing this paragraph and that human attention is needed under maintenance because the type system can't verify correctness. Even so, in our experience such "I know what I'm doing" situations are still a known bug source.

##### Exception

Consider:

    template<typename B>
    class Dx : B {
        // ...
    };

##### Enforcement

* Flag all uses of `static_cast` for downcasts, including C-style casts that perform a `static_cast`.
* This rule is part of the [type-safety profile](#Pro-type-downcast).

### <a name="Rh-ref-cast"></a>C.147: Use `dynamic_cast` to a reference type when failure to find the required class is considered an error

##### Reason

Casting to a reference expresses that you intend to end up with a valid object, so the cast must succeed. `dynamic_cast` will then throw if it does not succeed.

##### Example

    ???

##### Enforcement

???

### <a name="Rh-ptr-cast"></a>C.148: Use `dynamic_cast` to a pointer type when failure to find the required class is considered a valid alternative

##### Reason

The `dynamic_cast` conversion allows to test whether a pointer is pointing at a polymorphic object that has a given class in its hierarchy. Since failure to find the class merely returns a null value, it can be tested during run time. This allows writing code that can choose alternative paths depending on the results.

Contrast with [C.147](#Rh-ptr-cast), where failure is an error, and should not be used for conditional execution.

##### Example

The example below describes the `add` function of a `Shape_owner` that takes ownership of constructed `Shape` objects. The objects are also sorted into views, according to their geometric attributes.
In this example, `Shape` does not inherit from `Geometric_attributes`. Only its subclasses do.

    void add(Shape* const item)
    {
      // Ownership is always taken
      owned_shapes.emplace_back(item);

      // Check the Geometric_attributes and add the shape to none/one/some/all of the views

      if (auto even = dynamic_cast<Even_sided*>(item))
      {
        view_of_evens.emplace_back(even);
      }

      if (auto trisym = dynamic_cast<Trilaterally_symmetrical*>(item))
      {
        view_of_trisyms.emplace_back(trisym);
      }
    }

##### Notes

A failure to find the required class will cause `dynamic_cast` to return a null value, and de-referencing a null-valued pointer will lead to undefined behavior.
Therefore the result of the `dynamic_cast` should always be treated as if it may contain a null value, and tested.

##### Enforcement

* (Complex) Unless there is a null test on the result of a `dynamic_cast` of a pointer type, warn upon dereference of the pointer.

### <a name="Rh-smart"></a>C.149: Use `unique_ptr` or `shared_ptr` to avoid forgetting to `delete` objects created using `new`

##### Reason

Avoid resource leaks.

##### Example

    void use(int i)
    {
        auto p = new int {7};           // bad: initialize local pointers with new
        auto q = make_unique<int>(9);   // ok: guarantee the release of the memory-allocated for 9
        if (0 < i) return;              // maybe return and leak
        delete p;                       // too late
    }

##### Enforcement

* Flag initialization of a naked pointer with the result of a `new`
* Flag `delete` of local variable

### <a name="Rh-make_unique"></a>C.150: Use `make_unique()` to construct objects owned by `unique_ptr`s

##### Reason

 `make_unique` gives a more concise statement of the construction.
It also ensures exception safety in complex expressions.

##### Example

    unique_ptr<Foo> p {new<Foo>{7}};   // OK: but repetitive

    auto q = make_unique<Foo>(7);      // Better: no repetition of Foo

    // Not exception-safe: the compiler may interleave the computations of arguments as follows:
    //
    // 1. allocate memory for Foo,
    // 2. construct Foo,
    // 3. call bar,
    // 4. construct unique_ptr<Foo>.
    //
    // If bar throws, Foo will not be destroyed, and the memory-allocated for it will leak.
    f(unique_ptr<Foo>(new Foo()), bar());

    // Exception-safe: calls to functions are never interleaved.
    f(make_unique<Foo>(), bar());

##### Enforcement

* Flag the repetitive usage of template specialization list `<Foo>`
* Flag variables declared to be `unique_ptr<Foo>`

### <a name="Rh-make_shared"></a>C.151: Use `make_shared()` to construct objects owned by `shared_ptr`s

##### Reason

 `make_shared` gives a more concise statement of the construction.
It also gives an opportunity to eliminate a separate allocation for the reference counts, by placing the `shared_ptr`'s use counts next to its object.

##### Example

    void test() {
        // OK: but repetitive; and separate allocations for the Bar and shared_ptr's use count
        shared_ptr<Bar> p {new<Bar>{7}};

        auto q = make_shared<Bar>(7);   // Better: no repetition of Bar; one object
    }

##### Enforcement

* Flag the repetitive usage of template specialization list`<Bar>`
* Flag variables declared to be `shared_ptr<Bar>`

### <a name="Rh-array"></a>C.152: Never assign a pointer to an array of derived class objects to a pointer to its base

##### Reason

Subscripting the resulting base pointer will lead to invalid object access and probably to memory corruption.

##### Example

    struct B { int x; };
    struct D : B { int y; };

    void use(B*);

    D a[] = {{1, 2}, {3, 4}, {5, 6}};
    B* p = a;     // bad: a decays to &a[0] which is converted to a B*
    p[1].x = 7;   // overwrite D[0].y

    use(a);       // bad: a decays to &a[0] which is converted to a B*

##### Enforcement

* Flag all combinations of array decay and base to derived conversions.
* Pass an array as a `span` rather than as a pointer, and don't let the array name suffer a derived-to-base conversion before getting into the `span`


### <a name="Rh-use-virtual"></a>C.153: Prefer virtual function to casting

##### Reason

A virtual function call is safe, whereas casting is error-prone.
A virtual function call reaches the most derived function, whereas a cast may reach an intermediate class and therefore
give a wrong result (especially as a hierarchy is modified during maintenance).

##### Example

    ???

##### Enforcement

See [C.146](#Rh-dynamic_cast) and ???

## <a name="SS-overload"></a>C.over: Overloading and overloaded operators

You can overload ordinary functions, template functions, and operators.
You cannot overload function objects.

Overload rule summary:

* [C.160: Define operators primarily to mimic conventional usage](#Ro-conventional)
* [C.161: Use nonmember functions for symmetric operators](#Ro-symmetric)
* [C.162: Overload operations that are roughly equivalent](#Ro-equivalent)
* [C.163: Overload only for operations that are roughly equivalent](#Ro-equivalent-2)
* [C.164: Avoid implicit conversion operators](#Ro-conversion)
* [C.165: Use `using` for customization points](#Ro-custom)
* [C.166: Overload unary `&` only as part of a system of smart pointers and references](#Ro-address-of)
* [C.167: Use an operator for an operation with its conventional meaning](#Ro-overload)
* [C.168: Define overloaded operators in the namespace of their operands](#Ro-namespace)
* [C.170: If you feel like overloading a lambda, use a generic lambda](#Ro-lambda)

### <a name="Ro-conventional"></a>C.160: Define operators primarily to mimic conventional usage

##### Reason

Minimize surprises.

##### Example

    class X {
    public:
        // ...
        X& operator=(const X&); // member function defining assignment
        friend bool operator==(const X&, const X&); // == needs access to representation
                                                    // after a = b we have a == b
        // ...
    };

Here, the conventional semantics is maintained: [Copies compare equal](#SS-copy).

##### Example, bad

    X operator+(X a, X b) { return a.v - b.v; }   // bad: makes + subtract

##### Note

Nonmember operators should be either friends or defined in [the same namespace as their operands](#Ro-namespace).
[Binary operators should treat their operands equivalently](#Ro-symmetric).

##### Enforcement

Possibly impossible.

### <a name="Ro-symmetric"></a>C.161: Use nonmember functions for symmetric operators

##### Reason

If you use member functions, you need two.
Unless you use a nonmember function for (say) `==`, `a == b` and `b == a` will be subtly different.

##### Example

    bool operator==(Point a, Point b) { return a.x == b.x && a.y == b.y; }

##### Enforcement

Flag member operator functions.

### <a name="Ro-equivalent"></a>C.162: Overload operations that are roughly equivalent

##### Reason

Having different names for logically equivalent operations on different argument types is confusing, leads to encoding type information in function names, and inhibits generic programming.

##### Example

Consider:

    void print(int a);
    void print(int a, int base);
    void print(const string&);

These three functions all print their arguments (appropriately). Conversely:

    void print_int(int a);
    void print_based(int a, int base);
    void print_string(const string&);

These three functions all print their arguments (appropriately). Adding to the name just introduced verbosity and inhibits generic code.

##### Enforcement

???

### <a name="Ro-equivalent-2"></a>C.163: Overload only for operations that are roughly equivalent

##### Reason

Having the same name for logically different functions is confusing and leads to errors when using generic programming.

##### Example

Consider:

    void open_gate(Gate& g);   // remove obstacle from garage exit lane
    void fopen(const char* name, const char* mode);   // open file

The two operations are fundamentally different (and unrelated) so it is good that their names differ. Conversely:

    void open(Gate& g);   // remove obstacle from garage exit lane
    void open(const char* name, const char* mode ="r");   // open file

The two operations are still fundamentally different (and unrelated) but the names have been reduced to their (common) minimum, opening opportunities for confusion.
Fortunately, the type system will catch many such mistakes.

##### Note

Be particularly careful about common and popular names, such as `open`, `move`, `+`, and `==`.

##### Enforcement

???

### <a name="Ro-conversion"></a>C.164: Avoid implicit conversion operators

##### Reason

Implicit conversions can be essential (e.g., `double` to `int`) but often cause surprises (e.g., `String` to C-style string).

##### Note

Prefer explicitly named conversions until a serious need is demonstrated.
By "serious need" we mean a reason that is fundamental in the application domain (such as an integer to complex number conversion)
and frequently needed. Do not introduce implicit conversions (through conversion operators or non-`explicit` constructors)
just to gain a minor convenience.

##### Example

    struct S1 {
        string s;
        // ...
        operator char*() { return s.data(); }  // BAD, likely to cause surprises
    };

    struct S2 {
        string s;
        // ...
        explicit operator char*() { return s.data(); }
    };

    void f(S1 s1, S2 s2)
    {
        char* x1 = s1;     // OK, but can cause surprises in many contexts
        char* x2 = s2;     // error (and that's usually a good thing)
        char* x3 = static_cast<char*>(s2); // we can be explicit (on your head be it)
    }

The surprising and potentially damaging implicit conversion can occur in arbitrarily hard-to spot contexts, e.g.,

    S1 ff();

    char* g()
    {
        return ff();
    }

The string returned by `ff()` is destroyed before the returned pointer into it can be used.

##### Enforcement

Flag all conversion operators.

### <a name="Ro-custom"></a>C.165: Use `using` for customization points

##### Reason

To find function objects and functions defined in a separate namespace to "customize" a common function.

##### Example

Consider `swap`. It is a general (standard-library) function with a definition that will work for just about any type.
However, it is desirable to define specific `swap()`s for specific types.
For example, the general `swap()` will copy the elements of two `vector`s being swapped, whereas a good specific implementation will not copy elements at all.

    namespace N {
        My_type X { /* ... */ };
        void swap(X&, X&);   // optimized swap for N::X
        // ...
    }

    void f1(N::X& a, N::X& b)
    {
        std::swap(a, b);   // probably not what we wanted: calls std::swap()
    }

The `std::swap()` in `f1()` does exactly what we asked it to do: it calls the `swap()` in namespace `std`.
Unfortunately, that's probably not what we wanted.
How do we get `N::X` considered?

    void f2(N::X& a, N::X& b)
    {
        swap(a, b);   // calls N::swap
    }

But that may not be what we wanted for generic code.
There, we typically want the specific function if it exists and the general function if not.
This is done by including the general function in the lookup for the function:

    void f3(N::X& a, N::X& b)
    {
        using std::swap;  // make std::swap available
        swap(a, b);        // calls N::swap if it exists, otherwise std::swap
    }

##### Enforcement

Unlikely, except for known customization points, such as `swap`.
The problem is that the unqualified and qualified lookups both have uses.

### <a name="Ro-address-of"></a>C.166: Overload unary `&` only as part of a system of smart pointers and references

##### Reason

The `&` operator is fundamental in C++.
Many parts of the C++ semantics assumes its default meaning.

##### Example

    class Ptr { // a somewhat smart pointer
        Ptr(X* pp) :p(pp) { /* check */ }
        X* operator->() { /* check */ return p; }
        X operator[](int i);
        X operator*();
    private:
        T* p;
    };

    class X {
        Ptr operator&() { return Ptr{this}; }
        // ...
    };

##### Note

If you "mess with" operator `&` be sure that its definition has matching meanings for `->`, `[]`, `*`, and `.` on the result type.
Note that operator `.` currently cannot be overloaded so a perfect system is impossible.
We hope to remedy that: <http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4477.pdf>.
Note that `std::addressof()` always yields a built-in pointer.

##### Enforcement

Tricky. Warn if `&` is user-defined without also defining `->` for the result type.

### <a name="Ro-overload"></a>C.167: Use an operator for an operation with its conventional meaning

##### Reason

Readability. Convention. Reusability. Support for generic code

##### Example

    void cout_my_class(const My_class& c) // confusing, not conventional,not generic
    {
        std::cout << /* class members here */;
    }

    std::ostream& operator<<(std::ostream& os, const my_class& c) // OK
    {
        return os << /* class members here */;
    }

By itself, `cout_my_class` would be OK, but it is not usable/composable with code that rely on the `<<` convention for output:

    My_class var { /* ... */ };
    // ...
    cout << "var = " << var << '\n';

##### Note

There are strong and vigorous conventions for the meaning most operators, such as

* comparisons (`==`, `!=`, `<`, `<=`, `>`, and `>=`),
* arithmetic operations (`+`, `-`, `*`, `/`, and `%`)
* access operations (`.`, `->`, unary `*`, and `[]`)
* assignment (`=`)

Don't define those unconventionally and don't invent your own names for them.

##### Enforcement

Tricky. Requires semantic insight.

### <a name="Ro-namespace"></a>C.168: Define overloaded operators in the namespace of their operands

##### Reason

Readability.
Ability for find operators using ADL.
Avoiding inconsistent definition in different namespaces

##### Example

    struct S { };
    bool operator==(S, S);   // OK: in the same namespace as S, and even next to S
    S s;

    bool x = (s == s);

This is what a default `==` would do, if we had such defaults.

##### Example

    namespace N {
        struct S { };
        bool operator==(S, S);   // OK: in the same namespace as S, and even next to S
    }

    N::S s;

    bool x = (s == s);  // finds N::operator==() by ADL

##### Example, bad

    struct S { };
    S s;

    namespace N {
        S::operator!(S a) { return true; }
        S not_s = !s;
    }

    namespace M {
        S::operator!(S a) { return false; }
        S not_s = !s;
    }

Here, the meaning of `!s` differs in `N` and `M`.
This can be most confusing.
Remove the definition of `namespace M` and the confusion is replaced by an opportunity to make the mistake.

##### Note

If a binary operator is defined for two types that are defined in different namespaces, you cannot follow this rule.
For example:

    Vec::Vector operator*(const Vec::Vector&, const Mat::Matrix&);

This may be something best avoided.

##### See also

This is a special case of the rule that [helper functions should be defined in the same namespace as their class](#Rc-helper).

##### Enforcement

* Flag operator definitions that are not it the namespace of their operands

### <a name="Ro-lambda"></a>C.170: If you feel like overloading a lambda, use a generic lambda

##### Reason

You cannot overload by defining two different lambdas with the same name.

##### Example

    void f(int);
    void f(double);
    auto f = [](char);   // error: cannot overload variable and function

    auto g = [](int) { /* ... */ };
    auto g = [](double) { /* ... */ };   // error: cannot overload variables

    auto h = [](auto) { /* ... */ };   // OK

##### Enforcement

The compiler catches the attempt to overload a lambda.

## <a name="SS-union"></a>C.union: Unions

A `union` is a `struct` where all members start at the same address so that it can hold only one member at a time.
A `union` does not keep track of which member is stored so the programmer has to get it right;
this is inherently error-prone, but there are ways to compensate.

A type that is a `union` plus an indicator of which member is currently held is called a *tagged union*, a *discriminated union*, or a *variant*.

Union rule summary:

* [C.180: Use `union`s to save Memory](#Ru-union)
* [C.181: Avoid "naked" `union`s](#Ru-naked)
* [C.182: Use anonymous `union`s to implement tagged unions](#Ru-anonymous)
* [C.183: Don't use a `union` for type punning](#Ru-pun)
* ???

### <a name="Ru-union"></a>C.180: Use `union`s to save memory

##### Reason

A `union` allows a single piece of memory to be used for different types of objects at different times.
Consequently, it can be used to save memory when we have several objects that are never used at the same time.

##### Example

    union Value {
        int x;
        double d;
    };

    Value v = { 123 };  // now v holds an int
    cout << v.x << '\n';    // write 123
    v.d = 987.654;  // now v holds a double
    cout << v.d << '\n';    // write 987.654

But heed the warning: [Avoid "naked" `union`s](#Ru-naked)

##### Example

    // Short-string optimization

    constexpr size_t buffer_size = 16; // Slightly larger than the size of a pointer

    class Immutable_string {
    public:
        Immutable_string(const char* str) :
            size(strlen(str))
        {
            if (size < buffer_size)
                strcpy_s(string_buffer, buffer_size, str);
            else {
                string_ptr = new char[size + 1];
                strcpy_s(string_ptr, size + 1, str);
            }
        }

        ~Immutable_string()
        {
            if (size >= buffer_size)
                delete string_ptr;
        }

        const char* get_str() const
        {
            return (size < buffer_size) ? string_buffer : string_ptr;
        }

    private:
        // If the string is short enough, we store the string itself
        // instead of a pointer to the string.
        union {
            char* string_ptr;
            char string_buffer[buffer_size];
        };

        const size_t size;
    };

##### Enforcement

???

### <a name="Ru-naked"></a>C.181: Avoid "naked" `union`s

##### Reason

A *naked union* is a union without an associated indicator which member (if any) it holds,
so that the programmer has to keep track.
Naked unions are a source of type errors.

##### Example, bad

    union Value {
        int x;
        double d;
    };

    Value v;
    v.d = 987.654;  // v holds a double

So far, so good, but we can easily misuse the `union`:

    cout << v.x << '\n';    // BAD, undefined behavior: v holds a double, but we read it as an int

Note that the type error happened without any explicit cast.
When we tested that program the last value printed was `1683627180` which it the integer value for the bit pattern for `987.654`.
What we have here is an "invisible" type error that happens to give a result that could easily look innocent.

And, talking about "invisible", this code produced no output:

    v.x = 123;
    cout << v.d << '\n';    // BAD: undefined behavior

##### Alternative

Wrap a `union` in a class together with a type field.

The C++17 `variant` type (found in `<variant>`) does that for you:

    variant<int, double> v;
    v = 123;        // v holds an int
    int x = get<int>(v);
    v = 123.456;    // v holds a double
    w = get<double>(v);

##### Enforcement

???

### <a name="Ru-anonymous"></a>C.182: Use anonymous `union`s to implement tagged unions

##### Reason

A well-designed tagged union is type safe.
An *anonymous* union simplifies the definition of a class with a (tag, union) pair.

##### Example

This example is mostly borrowed from TC++PL4 pp216-218.
You can look there for an explanation.

The code is somewhat elaborate.
Handling a type with user-defined assignment and destructor is tricky.
Saving programmers from having to write such code is one reason for including `variant` in the standard.

    class Value { // two alternative representations represented as a union
    private:
        enum class Tag { number, text };
        Tag type; // discriminant

        union { // representation (note: anonymous union)
            int i;
            string s; // string has default constructor, copy operations, and destructor
        };
    public:
        struct Bad_entry { }; // used for exceptions

        ~Value();
        Value& operator=(const Value&);   // necessary because of the string variant
        Value(const Value&);
        // ...
        int number() const;
        string text() const;

        void set_number(int n);
        void set_text(const string&);
        // ...
    };

    int Value::number() const
    {
        if (type != Tag::number) throw Bad_entry{};
        return i;
    }

    string Value::text() const
    {
        if (type != Tag::text) throw Bad_entry{};
        return s;
    }

    void Value::set_number(int n)
    {
        if (type == Tag::text) {
            s.~string();      // explicitly destroy string
            type = Tag::number;
        }
        i = n;
    }

    void Value::set_text(const string& ss)
    {
        if (type == Tag::text)
            s = ss;
        else {
            new(&s) string{ss};   // placement new: explicitly construct string
            type = Tag::text;
        }
    }

    Value& Value::operator=(const Value& e)   // necessary because of the string variant
    {
        if (type == Tag::text && e.type == Tag::text) {
            s = e.s;    // usual string assignment
            return *this;
        }

        if (type == Tag::text) s.~string(); // explicit destroy

        switch (e.type) {
        case Tag::number:
            i = e.i;
            break;
        case Tag::text:
            new(&s) string(e.s);   // placement new: explicit construct
        }

        type = e.type;
        return *this;
    }

    Value::~Value()
    {
        if (type == Tag::text) s.~string(); // explicit destroy
    }

##### Enforcement

???

### <a name="Ru-pun"></a>C.183: Don't use a `union` for type punning

##### Reason

It is undefined behavior to read a `union` member with a different type from the one with which it was written.
Such punning is invisible, or at least harder to spot than using a named cast.
Type punning using a `union` is a source of errors.

##### Example, bad

    union Pun {
        int x;
        unsigned char c[sizeof(int)];
    };

The idea of `Pun` is to be able to look at the character representation of an `int`.

    void bad(Pun& u)
    {
        u.x = 'x';
        cout << u.c[0] << '\n';     // undefined behavior
    }

If you wanted to see the bytes of an `int`, use a (named) cast:

    void if_you_must_pun(int& x)
    {
        auto p = reinterpret_cast<unsigned char*>(&x);
        cout << p[0] << '\n';     // OK; better
        // ...
    }

Accessing the result of an `reinterpret_cast` to a different type from the objects declared type is defined behavior (even though `reinterpret_cast` is discouraged),
but at least we can see that something tricky is going on.

##### Note

Unfortunately, `union`s are commonly used for type punning.
We don't consider "sometimes, it works as expected" a strong argument.

C++17 introduced a distinct type `std::byte` to facilitate operations on raw object representation.  Use that type instead of `unsigned char` or `char` for these operations.

##### Enforcement

???



# <a name="S-enum"></a>Enum: Enumerations

Enumerations are used to define sets of integer values and for defining types for such sets of values.
There are two kind of enumerations, "plain" `enum`s and `class enum`s.

Enumeration rule summary:

* [Enum.1: Prefer enumerations over macros](#Renum-macro)
* [Enum.2: Use enumerations to represent sets of related named constants](#Renum-set)
* [Enum.3: Prefer `enum class`es over "plain" `enum`s](#Renum-class)
* [Enum.4: Define operations on enumerations for safe and simple use](#Renum-oper)
* [Enum.5: Don't use `ALL_CAPS` for enumerators](#Renum-caps)
* [Enum.6: Avoid unnamed enumerations](#Renum-unnamed)
* [Enum.7: Specify the underlying type of an enumeration only when necessary](#Renum-underlying)
* [Enum.8: Specify enumerator values only when necessary](#Renum-value)

### <a name="Renum-macro"></a>Enum.1: Prefer enumerations over macros

##### Reason

Macros do not obey scope and type rules. Also, macro names are removed during preprocessing and so usually don't appear in tools like debuggers.

##### Example

First some bad old code:

    // webcolors.h (third party header)
    #define RED   0xFF0000
    #define GREEN 0x00FF00
    #define BLUE  0x0000FF

    // productinfo.h
    // The following define product subtypes based on color
    #define RED    0
    #define PURPLE 1
    #define BLUE   2

    int webby = BLUE;   // webby == 2; probably not what was desired

Instead use an `enum`:

    enum class Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };
    enum class Product_info { red = 0, purple = 1, blue = 2 };

    int webby = blue;   // error: be specific
    Web_color webby = Web_color::blue;

We used an `enum class` to avoid name clashes.

##### Enforcement

Flag macros that define integer values.


### <a name="Renum-set"></a>Enum.2: Use enumerations to represent sets of related named constants

##### Reason

An enumeration shows the enumerators to be related and can be a named type.



##### Example

    enum class Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };


##### Note

Switching on an enumeration is common and the compiler can warn against unusual patterns of case labels. For example:

    enum class Product_info { red = 0, purple = 1, blue = 2 };

    void print(Product_info inf)
    {
        switch (inf) {
        case Product_info::red: cout << "red"; break;
        case Product_info::purple: cout << "purple"; break;
        }
    }

Such off-by-one switch`statements are often the results of an added enumerator and insufficient testing.

##### Enforcement

* Flag `switch`-statements where the `case`s cover most but not all enumerators of an enumeration.
* Flag `switch`-statements where the `case`s cover a few enumerators of an enumeration, but has no `default`.


### <a name="Renum-class"></a>Enum.3: Prefer class enums over "plain" enums

##### Reason

To minimize surprises: traditional enums convert to int too readily.

##### Example

    void Print_color(int color);

    enum Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };
    enum Product_info { Red = 0, Purple = 1, Blue = 2 };

    Web_color webby = Web_color::blue;

    // Clearly at least one of these calls is buggy.
    Print_color(webby);
    Print_color(Product_info::Blue);

Instead use an `enum class`:

    void Print_color(int color);

    enum class Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };
    enum class Product_info { red = 0, purple = 1, blue = 2 };

    Web_color webby = Web_color::blue;
    Print_color(webby);  // Error: cannot convert Web_color to int.
    Print_color(Product_info::Red);  // Error: cannot convert Product_info to int.

##### Enforcement

(Simple) Warn on any non-class `enum` definition.

### <a name="Renum-oper"></a>Enum.4: Define operations on enumerations for safe and simple use

##### Reason

Convenience of use and avoidance of errors.

##### Example

    enum Day { mon, tue, wed, thu, fri, sat, sun };

    Day& operator++(Day& d)
    {
        return d = (d == Day::sun) ? Day::mon : static_cast<Day>(static_cast<int>(d)+1);
    }

    Day today = Day::sat;
    Day tomorrow = ++today;

The use of a `static_cast` is not pretty, but

    Day& operator++(Day& d)
    {
        return d = (d == Day::sun) ? Day::mon : Day{++d};    // error
    }

is an infinite recursion, and writing it without a cast, using a `switch` on all cases is long-winded.


##### Enforcement

Flag repeated expressions cast back into an enumeration.


### <a name="Renum-caps"></a>Enum.5: Don't use `ALL_CAPS` for enumerators

##### Reason

Avoid clashes with macros.

##### Example, bad

     // webcolors.h (third party header)
    #define RED   0xFF0000
    #define GREEN 0x00FF00
    #define BLUE  0x0000FF

    // productinfo.h
    // The following define product subtypes based on color

    enum class Product_info { RED, PURPLE, BLUE };   // syntax error

##### Enforcement

Flag ALL_CAPS enumerators.

### <a name="Renum-unnamed"></a>Enum.6: Avoid unnamed enumerations

##### Reason

If you can't name an enumeration, the values are not related

##### Example, bad

    enum { red = 0xFF0000, scale = 4, is_signed = 1 };

Such code is not uncommon in code written before there were convenient alternative ways of specifying integer constants.

##### Alternative

Use `constexpr` values instead. For example:

    constexpr int red = 0xFF0000;
    constexpr short scale = 4;
    constexpr bool is_signed = true;

##### Enforcement

Flag unnamed enumerations.


### <a name="Renum-underlying"></a>Enum.7: Specify the underlying type of an enumeration only when necessary

##### Reason

The default is the easiest to read and write.
`int` is the default integer type.
`int` is compatible with C `enum`s.

##### Example

    enum class Direction : char { n, s, e, w,
                                  ne, nw, se, sw };  // underlying type saves space

    enum class Web_color : int32_t { red   = 0xFF0000,
                                     green = 0x00FF00,
                                     blue  = 0x0000FF };  // underlying type is redundant

##### Note

Specifying the underlying type is necessary in forward declarations of enumerations:

    enum Flags : char;

    void f(Flags);

    // ....

    enum flags : char { /* ... */ };


##### Enforcement

????


### <a name="Renum-value"></a>Enum.8: Specify enumerator values only when necessary

##### Reason

It's the simplest.
It avoids duplicate enumerator values.
The default gives a consecutive set of values that is good for `switch`-statement implementations.

##### Example

    enum class Col1 { red, yellow, blue };
    enum class Col2 { red = 1, yellow = 2, blue = 2 }; // typo
    enum class Month { jan = 1, feb, mar, apr, may, jun,
                       jul, august, sep, oct, nov, dec }; // starting with 1 is conventional
    enum class Base_flag { dec = 1, oct = dec << 1, hex = dec << 2 }; // set of bits

Specifying values is necessary to match conventional values (e.g., `Month`)
and where consecutive values are undesirable (e.g., to get separate bits as in `Base_flag`).

##### Enforcement

* Flag duplicate enumerator values
* Flag explicitly specified all-consecutive enumerator values


# <a name="S-resource"></a>R: Resource management

This section contains rules related to resources.
A resource is anything that must be acquired and (explicitly or implicitly) released, such as memory, file handles, sockets, and locks.
The reason it must be released is typically that it can be in short supply, so even delayed release may do harm.
The fundamental aim is to ensure that we don't leak any resources and that we don't hold a resource longer than we need to.
An entity that is responsible for releasing a resource is called an owner.

There are a few cases where leaks can be acceptable or even optimal:
If you are writing a program that simply produces an output based on an input and the amount of memory needed is proportional to the size of the input, the optimal strategy (for performance and ease of programming) is sometimes simply never to delete anything.
If you have enough memory to handle your largest input, leak away, but be sure to give a good error message if you are wrong.
Here, we ignore such cases.

* Resource management rule summary:

  * [R.1: Manage resources automatically using resource handles and RAII (Resource Acquisition Is Initialization)](#Rr-raii)
  * [R.2: In interfaces, use raw pointers to denote individual objects (only)](#Rr-use-ptr)
  * [R.3: A raw pointer (a `T*`) is non-owning](#Rr-ptr)
  * [R.4: A raw reference (a `T&`) is non-owning](#Rr-ref)
  * [R.5: Prefer scoped objects, don't heap-allocate unnecessarily](#Rr-scoped)
  * [R.6: Avoid non-`const` global variables](#Rr-global)

* Allocation and deallocation rule summary:

  * [R.10: Avoid `malloc()` and `free()`](#Rr-mallocfree)
  * [R.11: Avoid calling `new` and `delete` explicitly](#Rr-newdelete)
  * [R.12: Immediately give the result of an explicit resource allocation to a manager object](#Rr-immediate-alloc)
  * [R.13: Perform at most one explicit resource allocation in a single expression statement](#Rr-single-alloc)
  * [R.14: Avoid `[]` parameters, prefer `span`](#Rr-ap)
  * [R.15: Always overload matched allocation/deallocation pairs](#Rr-pair)

* <a name="Rr-summary-smartptrs"></a>Smart pointer rule summary:

  * [R.20: Use `unique_ptr` or `shared_ptr` to represent ownership](#Rr-owner)
  * [R.21: Prefer `unique_ptr` over `shared_ptr` unless you need to share ownership](#Rr-unique)
  * [R.22: Use `make_shared()` to make `shared_ptr`s](#Rr-make_shared)
  * [R.23: Use `make_unique()` to make `unique_ptr`s](#Rr-make_unique)
  * [R.24: Use `std::weak_ptr` to break cycles of `shared_ptr`s](#Rr-weak_ptr)
  * [R.30: Take smart pointers as parameters only to explicitly express lifetime semantics](#Rr-smartptrparam)
  * [R.31: If you have non-`std` smart pointers, follow the basic pattern from `std`](#Rr-smart)
  * [R.32: Take a `unique_ptr<widget>` parameter to express that a function assumes ownership of a `widget`](#Rr-uniqueptrparam)
  * [R.33: Take a `unique_ptr<widget>&` parameter to express that a function reseats the `widget`](#Rr-reseat)
  * [R.34: Take a `shared_ptr<widget>` parameter to express that a function is part owner](#Rr-sharedptrparam-owner)
  * [R.35: Take a `shared_ptr<widget>&` parameter to express that a function might reseat the shared pointer](#Rr-sharedptrparam)
  * [R.36: Take a `const shared_ptr<widget>&` parameter to express that it might retain a reference count to the object ???](#Rr-sharedptrparam-const)
  * [R.37: Do not pass a pointer or reference obtained from an aliased smart pointer](#Rr-smartptrget)

### <a name="Rr-raii"></a>R.1: Manage resources automatically using resource handles and RAII (Resource Acquisition Is Initialization)

##### Reason

To avoid leaks and the complexity of manual resource management.
C++'s language-enforced constructor/destructor symmetry mirrors the symmetry inherent in resource acquire/release function pairs such as `fopen`/`fclose`, `lock`/`unlock`, and `new`/`delete`.
Whenever you deal with a resource that needs paired acquire/release function calls, encapsulate that resource in an object that enforces pairing for you -- acquire the resource in its constructor, and release it in its destructor.

##### Example, bad

Consider:

    void send(X* x, cstring_span destination)
    {
        auto port = open_port(destination);
        my_mutex.lock();
        // ...
        send(port, x);
        // ...
        my_mutex.unlock();
        close_port(port);
        delete x;
    }

In this code, you have to remember to `unlock`, `close_port`, and `delete` on all paths, and do each exactly once.
Further, if any of the code marked `...` throws an exception, then `x` is leaked and `my_mutex` remains locked.

##### Example

Consider:

    void send(unique_ptr<X> x, cstring_span destination)  // x owns the X
    {
        Port port{destination};            // port owns the PortHandle
        lock_guard<mutex> guard{my_mutex}; // guard owns the lock
        // ...
        send(port, x);
        // ...
    } // automatically unlocks my_mutex and deletes the pointer in x

Now all resource cleanup is automatic, performed once on all paths whether or not there is an exception. As a bonus, the function now advertises that it takes over ownership of the pointer.

What is `Port`? A handy wrapper that encapsulates the resource:

    class Port {
        PortHandle port;
    public:
        Port(cstring_span destination) : port{open_port(destination)} { }
        ~Port() { close_port(port); }
        operator PortHandle() { return port; }

        // port handles can't usually be cloned, so disable copying and assignment if necessary
        Port(const Port&) = delete;
        Port& operator=(const Port&) = delete;
    };

##### Note

Where a resource is "ill-behaved" in that it isn't represented as a class with a destructor, wrap it in a class or use [`finally`](#Re-finally)

**See also**: [RAII](#Rr-raii)

### <a name="Rr-use-ptr"></a>R.2: In interfaces, use raw pointers to denote individual objects (only)

##### Reason

Arrays are best represented by a container type (e.g., `vector` (owning)) or a `span` (non-owning).
Such containers and views hold sufficient information to do range checking.

##### Example, bad

    void f(int* p, int n)   // n is the number of elements in p[]
    {
        // ...
        p[2] = 7;   // bad: subscript raw pointer
        // ...
    }

The compiler does not read comments, and without reading other code you do not know whether `p` really points to `n` elements.
Use a `span` instead.

##### Example

    void g(int* p, int fmt)   // print *p using format #fmt
    {
        // ... uses *p and p[0] only ...
    }

##### Exception

C-style strings are passed as single pointers to a zero-terminated sequence of characters.
Use `zstring` rather than `char*` to indicate that you rely on that convention.

##### Note

Many current uses of pointers to a single element could be references.
However, where `nullptr` is a possible value, a reference may not be a reasonable alternative.

##### Enforcement

* Flag pointer arithmetic (including `++`) on a pointer that is not part of a container, view, or iterator.
  This rule would generate a huge number of false positives if applied to an older code base.
* Flag array names passed as simple pointers

### <a name="Rr-ptr"></a>R.3: A raw pointer (a `T*`) is non-owning

##### Reason

There is nothing (in the C++ standard or in most code) to say otherwise and most raw pointers are non-owning.
We want owning pointers identified so that we can reliably and efficiently delete the objects pointed to by owning pointers.

##### Example

    void f()
    {
        int* p1 = new int{7};           // bad: raw owning pointer
        auto p2 = make_unique<int>(7);  // OK: the int is owned by a unique pointer
        // ...
    }

The `unique_ptr` protects against leaks by guaranteeing the deletion of its object (even in the presence of exceptions). The `T*` does not.

##### Example

    template<typename T>
    class X {
        // ...
    public:
        T* p;   // bad: it is unclear whether p is owning or not
        T* q;   // bad: it is unclear whether q is owning or not
    };

We can fix that problem by making ownership explicit:

    template<typename T>
    class X2 {
        // ...
    public:
        owner<T*> p;  // OK: p is owning
        T* q;         // OK: q is not owning
    };

##### Exception

A major class of exception is legacy code, especially code that must remain compilable as C or interface with C and C-style C++ through ABIs.
The fact that there are billions of lines of code that violate this rule against owning `T*`s cannot be ignored.
We'd love to see program transformation tools turning 20-year-old "legacy" code into shiny modern code,
we encourage the development, deployment and use of such tools,
we hope the guidelines will help the development of such tools,
and we even contributed (and contribute) to the research and development in this area.
However, it will take time: "legacy code" is generated faster than we can renovate old code, and so it will be for a few years.

This code cannot all be rewritten (ever assuming good code transformation software), especially not soon.
This problem cannot be solved (at scale) by transforming all owning pointers to `unique_ptr`s and `shared_ptr`s,
partly because we need/use owning "raw pointers" as well as simple pointers in the implementation of our fundamental resource handles.
For example, common `vector` implementations have one owning pointer and two non-owning pointers.
Many ABIs (and essentially all interfaces to C code) use `T*`s, some of them owning.
Some interfaces cannot be simply annotated with `owner` because they need to remain compilable as C
(although this would be a rare good use for a macro, that expands to `owner` in C++ mode only).

##### Note

`owner<T*>` has no default semantics beyond `T*`. It can be used without changing any code using it and without affecting ABIs.
It is simply a indicator to programmers and analysis tools.
For example, if an `owner<T*>` is a member of a class, that class better have a destructor that `delete`s it.

##### Example, bad

Returning a (raw) pointer imposes a lifetime management uncertainty on the caller; that is, who deletes the pointed-to object?

    Gadget* make_gadget(int n)
    {
        auto p = new Gadget{n};
        // ...
        return p;
    }

    void caller(int n)
    {
        auto p = make_gadget(n);   // remember to delete p
        // ...
        delete p;
    }

In addition to suffering from the problem from [leak](#???), this adds a spurious allocation and deallocation operation, and is needlessly verbose. If Gadget is cheap to move out of a function (i.e., is small or has an efficient move operation), just return it "by value" (see ["out" return values](#Rf-out)):

    Gadget make_gadget(int n)
    {
        Gadget g{n};
        // ...
        return g;
    }

##### Note

This rule applies to factory functions.

##### Note

If pointer semantics are required (e.g., because the return type needs to refer to a base class of a class hierarchy (an interface)), return a "smart pointer."

##### Enforcement

* (Simple) Warn on `delete` of a raw pointer that is not an `owner<T>`.
* (Moderate) Warn on failure to either `reset` or explicitly `delete` an `owner<T>` pointer on every code path.
* (Simple) Warn if the return value of `new` is assigned to a raw pointer.
* (Simple) Warn if a function returns an object that was allocated within the function but has a move constructor.
  Suggest considering returning it by value instead.

### <a name="Rr-ref"></a>R.4: A raw reference (a `T&`) is non-owning

##### Reason

There is nothing (in the C++ standard or in most code) to say otherwise and most raw references are non-owning.
We want owners identified so that we can reliably and efficiently delete the objects pointed to by owning pointers.

##### Example

    void f()
    {
        int& r = *new int{7};  // bad: raw owning reference
        // ...
        delete &r;             // bad: violated the rule against deleting raw pointers
    }

**See also**: [The raw pointer rule](#Rr-ptr)

##### Enforcement

See [the raw pointer rule](#Rr-ptr)

### <a name="Rr-scoped"></a>R.5: Prefer scoped objects, don't heap-allocate unnecessarily

##### Reason

A scoped object is a local object, a global object, or a member.
This implies that there is no separate allocation and deallocation cost in excess of that already used for the containing scope or object.
The members of a scoped object are themselves scoped and the scoped object's constructor and destructor manage the members' lifetimes.

##### Example

The following example is inefficient (because it has unnecessary allocation and deallocation), vulnerable to exception throws and returns in the `...` part (leading to leaks), and verbose:

    void f(int n)
    {
        auto p = new Gadget{n};
        // ...
        delete p;
    }

Instead, use a local variable:

    void f(int n)
    {
        Gadget g{n};
        // ...
    }

##### Enforcement

* (Moderate) Warn if an object is allocated and then deallocated on all paths within a function. Suggest it should be a local `auto` stack object instead.
* (Simple) Warn if a local `Unique_ptr` or `Shared_ptr` is not moved, copied, reassigned or `reset` before its lifetime ends.

### <a name="Rr-global"></a>R.6: Avoid non-`const` global variables

##### Reason

Global variables can be accessed from everywhere so they can introduce surprising dependencies between apparently unrelated objects.
They are a notable source of errors.

**Warning**: The initialization of global objects is not totally ordered.
If you use a global object initialize it with a constant.
Note that it is possible to get undefined initialization order even for `const` objects.

##### Exception

A global object is often better than a singleton.

##### Exception

An immutable (`const`) global does not introduce the problems we try to avoid by banning global objects.

##### Enforcement

(??? NM: Obviously we can warn about non-`const` statics ... do we want to?)

## <a name="SS-alloc"></a>R.alloc: Allocation and deallocation

### <a name="Rr-mallocfree"></a>R.10: Avoid `malloc()` and `free()`

##### Reason

 `malloc()` and `free()` do not support construction and destruction, and do not mix well with `new` and `delete`.

##### Example

    class Record {
        int id;
        string name;
        // ...
    };

    void use()
    {
        // p1 may be nullptr
        // *p1 is not initialized; in particular,
        // that string isn't a string, but a string-sized bag of bits
        Record* p1 = static_cast<Record*>(malloc(sizeof(Record)));

        auto p2 = new Record;

        // unless an exception is thrown, *p2 is default initialized
        auto p3 = new(nothrow) Record;
        // p3 may be nullptr; if not, *p3 is default initialized

        // ...

        delete p1;    // error: cannot delete object allocated by malloc()
        free(p2);    // error: cannot free() object allocated by new
    }

In some implementations that `delete` and that `free()` might work, or maybe they will cause run-time errors.

##### Exception

There are applications and sections of code where exceptions are not acceptable.
Some of the best such examples are in life-critical hard-real-time code.
Beware that many bans on exception use are based on superstition (bad)
or by concerns for older code bases with unsystematic resource management (unfortunately, but sometimes necessary).
In such cases, consider the `nothrow` versions of `new`.

##### Enforcement

Flag explicit use of `malloc` and `free`.

### <a name="Rr-newdelete"></a>R.11: Avoid calling `new` and `delete` explicitly

##### Reason

The pointer returned by `new` should belong to a resource handle (that can call `delete`).
If the pointer returned by `new` is assigned to a plain/naked pointer, the object can be leaked.

##### Note

In a large program, a naked `delete` (that is a `delete` in application code, rather than part of code devoted to resource management)
is a likely bug: if you have N `delete`s, how can you be certain that you don't need N+1 or N-1?
The bug may be latent: it may emerge only during maintenance.
If you have a naked `new`, you probably need a naked `delete` somewhere, so you probably have a bug.

##### Enforcement

(Simple) Warn on any explicit use of `new` and `delete`. Suggest using `make_unique` instead.

### <a name="Rr-immediate-alloc"></a>R.12: Immediately give the result of an explicit resource allocation to a manager object

##### Reason

If you don't, an exception or a return may lead to a leak.

##### Example, bad

    void f(const string& name)
    {
        FILE* f = fopen(name, "r");            // open the file
        vector<char> buf(1024);
        auto _ = finally([f] { fclose(f); });  // remember to close the file
        // ...
    }

The allocation of `buf` may fail and leak the file handle.

##### Example

    void f(const string& name)
    {
        ifstream f{name};   // open the file
        vector<char> buf(1024);
        // ...
    }

The use of the file handle (in `ifstream`) is simple, efficient, and safe.

##### Enforcement

* Flag explicit allocations used to initialize pointers (problem: how many direct resource allocations can we recognize?)

### <a name="Rr-single-alloc"></a>R.13: Perform at most one explicit resource allocation in a single expression statement

##### Reason

If you perform two explicit resource allocations in one statement, you could leak resources because the order of evaluation of many subexpressions, including function arguments, is unspecified.

##### Example

    void fun(shared_ptr<Widget> sp1, shared_ptr<Widget> sp2);

This `fun` can be called like this:

    // BAD: potential leak
    fun(shared_ptr<Widget>(new Widget(a, b)), shared_ptr<Widget>(new Widget(c, d)));

This is exception-unsafe because the compiler may reorder the two expressions building the function's two arguments.
In particular, the compiler can interleave execution of the two expressions:
Memory allocation (by calling `operator new`) could be done first for both objects, followed by attempts to call the two `Widget` constructors.
If one of the constructor calls throws an exception, then the other object's memory will never be released!

This subtle problem has a simple solution: Never perform more than one explicit resource allocation in a single expression statement.
For example:

    shared_ptr<Widget> sp1(new Widget(a, b)); // Better, but messy
    fun(sp1, new Widget(c, d));

The best solution is to avoid explicit allocation entirely use factory functions that return owning objects:

    fun(make_shared<Widget>(a, b), make_shared<Widget>(c, d)); // Best

Write your own factory wrapper if there is not one already.

##### Enforcement

* Flag expressions with multiple explicit resource allocations (problem: how many direct resource allocations can we recognize?)

### <a name="Rr-ap"></a>R.14: Avoid `[]` parameters, prefer `span`

##### Reason

An array decays to a pointer, thereby losing its size, opening the opportunity for range errors.
Use `span` to preserve size information.

##### Example

    void f(int[]);          // not recommended
    
    void f(int*);           // not recommended for multiple objects
                            // (a pointer should point to a single object, do not subscript)

    void f(gsl::span<int>); // good, recommended

##### Enforcement

Flag `[]` parameters. Use `span` instead.

### <a name="Rr-pair"></a>R.15: Always overload matched allocation/deallocation pairs

##### Reason

Otherwise you get mismatched operations and chaos.

##### Example

    class X {
        // ...
        void* operator new(size_t s);
        void operator delete(void*);
        // ...
    };

##### Note

If you want memory that cannot be deallocated, `=delete` the deallocation operation.
Don't leave it undeclared.

##### Enforcement

Flag incomplete pairs.

## <a name="SS-smart"></a>R.smart: Smart pointers

### <a name="Rr-owner"></a>R.20: Use `unique_ptr` or `shared_ptr` to represent ownership

##### Reason

They can prevent resource leaks.

##### Example

Consider:

    void f()
    {
        X x;
        X* p1 { new X };              // see also ???
        unique_ptr<T> p2 { new X };   // unique ownership; see also ???
        shared_ptr<T> p3 { new X };   // shared ownership; see also ???
        auto p4 = make_unique<X>();   // unique_ownership, preferable to the explicit use "new"
        auto p5 = make_shared<X>();   // shared ownership, preferable to the explicit use "new"
    }

This will leak the object used to initialize `p1` (only).

##### Enforcement

(Simple) Warn if the return value of `new` or a function call with return value of pointer type is assigned to a raw pointer.

### <a name="Rr-unique"></a>R.21: Prefer `unique_ptr` over `shared_ptr` unless you need to share ownership

##### Reason

A `unique_ptr` is conceptually simpler and more predictable (you know when destruction happens) and faster (you don't implicitly maintain a use count).

##### Example, bad

This needlessly adds and maintains a reference count.

    void f()
    {
        shared_ptr<Base> base = make_shared<Derived>();
        // use base locally, without copying it -- refcount never exceeds 1
    } // destroy base

##### Example

This is more efficient:

    void f()
    {
        unique_ptr<Base> base = make_unique<Derived>();
        // use base locally
    } // destroy base

##### Enforcement

(Simple) Warn if a function uses a `Shared_ptr` with an object allocated within the function, but never returns the `Shared_ptr` or passes it to a function requiring a `Shared_ptr&`. Suggest using `unique_ptr` instead.

### <a name="Rr-make_shared"></a>R.22: Use `make_shared()` to make `shared_ptr`s

##### Reason

If you first make an object and then give it to a `shared_ptr` constructor, you (most likely) do one more allocation (and later deallocation) than if you use `make_shared()` because the reference counts must be allocated separately from the object.

##### Example

Consider:

    shared_ptr<X> p1 { new X{2} }; // bad
    auto p = make_shared<X>(2);    // good

The `make_shared()` version mentions `X` only once, so it is usually shorter (as well as faster) than the version with the explicit `new`.

##### Enforcement

(Simple) Warn if a `shared_ptr` is constructed from the result of `new` rather than `make_shared`.

### <a name="Rr-make_unique"></a>R.23: Use `make_unique()` to make `unique_ptr`s

##### Reason

For convenience and consistency with `shared_ptr`.

##### Note

`make_unique()` is C++14, but widely available (as well as simple to write).

##### Enforcement

(Simple) Warn if a `unique_ptr` is constructed from the result of `new` rather than `make_unique`.

### <a name="Rr-weak_ptr"></a>R.24: Use `std::weak_ptr` to break cycles of `shared_ptr`s

##### Reason

 `shared_ptr`'s rely on use counting and the use count for a cyclic structure never goes to zero, so we need a mechanism to
be able to destroy a cyclic structure.

##### Example

    #include <memory>

    class bar;

    class foo
    {
    public:
      explicit foo(const std::shared_ptr<bar>& forward_reference)
        : forward_reference_(forward_reference)
      { }
    private:
      std::shared_ptr<bar> forward_reference_;
    };

    class bar
    {
    public:
      explicit bar(const std::weak_ptr<foo>& back_reference)
        : back_reference_(back_reference)
      { }
      void do_something()
      {
        if (auto shared_back_reference = back_reference_.lock()) {
          // Use *shared_back_reference
        }
      }
    private:
      std::weak_ptr<foo> back_reference_;
    };

##### Note

 ??? (HS: A lot of people say "to break cycles", while I think "temporary shared ownership" is more to the point.)
???(BS: breaking cycles is what you must do; temporarily sharing ownership is how you do it.
You could "temporarily share ownership" simply by using another `shared_ptr`.)

##### Enforcement

??? probably impossible. If we could statically detect cycles, we wouldn't need `weak_ptr`

### <a name="Rr-smartptrparam"></a>R.30: Take smart pointers as parameters only to explicitly express lifetime semantics

##### Reason

Accepting a smart pointer to a `widget` is wrong if the function just needs the `widget` itself.
It should be able to accept any `widget` object, not just ones whose lifetimes are managed by a particular kind of smart pointer.
A function that does not manipulate lifetime should take raw pointers or references instead.

##### Example, bad

    // callee
    void f(shared_ptr<widget>& w)
    {
        // ...
        use(*w); // only use of w -- the lifetime is not used at all
        // ...
    };

    // caller
    shared_ptr<widget> my_widget = /* ... */;
    f(my_widget);

    widget stack_widget;
    f(stack_widget); // error

##### Example, good

    // callee
    void f(widget& w)
    {
        // ...
        use(w);
        // ...
    };

    // caller
    shared_ptr<widget> my_widget = /* ... */;
    f(*my_widget);

    widget stack_widget;
    f(stack_widget); // ok -- now this works

##### Enforcement

* (Simple) Warn if a function takes a parameter of a smart pointer type (that overloads `operator->` or `operator*`) that is copyable but the function only calls any of: `operator*`, `operator->` or `get()`.
  Suggest using a `T*` or `T&` instead.
* Flag a parameter of a smart pointer type (a type that overloads `operator->` or `operator*`) that is copyable/movable but never copied/moved from in the function body, and that is never modified, and that is not passed along to another function that could do so. That means the ownership semantics are not used.
  Suggest using a `T*` or `T&` instead.

### <a name="Rr-smart"></a>R.31: If you have non-`std` smart pointers, follow the basic pattern from `std`

##### Reason

The rules in the following section also work for other kinds of third-party and custom smart pointers and are very useful for diagnosing common smart pointer errors that cause performance and correctness problems.
You want the rules to work on all the smart pointers you use.

Any type (including primary template or specialization) that overloads unary `*` and `->` is considered a smart pointer:

* If it is copyable, it is recognized as a reference-counted `shared_ptr`.
* If it is not copyable, it is recognized as a unique `unique_ptr`.

##### Example

    // use Boost's intrusive_ptr
    #include <boost/intrusive_ptr.hpp>
    void f(boost::intrusive_ptr<widget> p)  // error under rule 'sharedptrparam'
    {
        p->foo();
    }

    // use Microsoft's CComPtr
    #include <atlbase.h>
    void f(CComPtr<widget> p)               // error under rule 'sharedptrparam'
    {
        p->foo();
    }

Both cases are an error under the [`sharedptrparam` guideline](#Rr-smartptrparam):
`p` is a `Shared_ptr`, but nothing about its sharedness is used here and passing it by value is a silent pessimization;
these functions should accept a smart pointer only if they need to participate in the widget's lifetime management. Otherwise they should accept a `widget*`, if it can be `nullptr`. Otherwise, and ideally, the function should accept a `widget&`.
These smart pointers match the `Shared_ptr` concept, so these guideline enforcement rules work on them out of the box and expose this common pessimization.

### <a name="Rr-uniqueptrparam"></a>R.32: Take a `unique_ptr<widget>` parameter to express that a function assumes ownership of a `widget`

##### Reason

Using `unique_ptr` in this way both documents and enforces the function call's ownership transfer.

##### Example

    void sink(unique_ptr<widget>); // takes ownership of the widget

    void uses(widget*);            // just uses the widget

##### Example, bad

    void thinko(const unique_ptr<widget>&); // usually not what you want

##### Enforcement

* (Simple) Warn if a function takes a `Unique_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Unique_ptr<T>` parameter by reference to `const`. Suggest taking a `const T*` or `const T&` instead.

### <a name="Rr-reseat"></a>R.33: Take a `unique_ptr<widget>&` parameter to express that a function reseats the`widget`

##### Reason

Using `unique_ptr` in this way both documents and enforces the function call's reseating semantics.

##### Note

"reseat" means "making a pointer or a smart pointer refer to a different object."

##### Example

    void reseat(unique_ptr<widget>&); // "will" or "might" reseat pointer

##### Example, bad

    void thinko(const unique_ptr<widget>&); // usually not what you want

##### Enforcement

* (Simple) Warn if a function takes a `Unique_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Unique_ptr<T>` parameter by reference to `const`. Suggest taking a `const T*` or `const T&` instead.

### <a name="Rr-sharedptrparam-owner"></a>R.34: Take a `shared_ptr<widget>` parameter to express that a function is part owner

##### Reason

This makes the function's ownership sharing explicit.

##### Example, good

    void share(shared_ptr<widget>);            // share -- "will" retain refcount

    void may_share(const shared_ptr<widget>&); // "might" retain refcount

    void reseat(shared_ptr<widget>&);          // "might" reseat ptr

##### Enforcement

* (Simple) Warn if a function takes a `Shared_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by value or by reference to `const` and does not copy or move it to another `Shared_ptr` on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by rvalue reference. Suggesting taking it by value instead.

### <a name="Rr-sharedptrparam"></a>R.35: Take a `shared_ptr<widget>&` parameter to express that a function might reseat the shared pointer

##### Reason

This makes the function's reseating explicit.

##### Note

"reseat" means "making a reference or a smart pointer refer to a different object."

##### Example, good

    void share(shared_ptr<widget>);            // share -- "will" retain refcount

    void reseat(shared_ptr<widget>&);          // "might" reseat ptr

    void may_share(const shared_ptr<widget>&); // "might" retain refcount

##### Enforcement

* (Simple) Warn if a function takes a `Shared_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by value or by reference to `const` and does not copy or move it to another `Shared_ptr` on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by rvalue reference. Suggesting taking it by value instead.

### <a name="Rr-sharedptrparam-const"></a>R.36: Take a `const shared_ptr<widget>&` parameter to express that it might retain a reference count to the object ???

##### Reason

This makes the function's ??? explicit.

##### Example, good

    void share(shared_ptr<widget>);            // share -- "will" retain refcount

    void reseat(shared_ptr<widget>&);          // "might" reseat ptr

    void may_share(const shared_ptr<widget>&); // "might" retain refcount

##### Enforcement

* (Simple) Warn if a function takes a `Shared_ptr<T>` parameter by lvalue reference and does not either assign to it or call `reset()` on it on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by value or by reference to `const` and does not copy or move it to another `Shared_ptr` on at least one code path. Suggest taking a `T*` or `T&` instead.
* (Simple) ((Foundation)) Warn if a function takes a `Shared_ptr<T>` by rvalue reference. Suggesting taking it by value instead.

### <a name="Rr-smartptrget"></a>R.37: Do not pass a pointer or reference obtained from an aliased smart pointer

##### Reason

Violating this rule is the number one cause of losing reference counts and finding yourself with a dangling pointer.
Functions should prefer to pass raw pointers and references down call chains.
At the top of the call tree where you obtain the raw pointer or reference from a smart pointer that keeps the object alive.
You need to be sure that the smart pointer cannot inadvertently be reset or reassigned from within the call tree below.

##### Note

To do this, sometimes you need to take a local copy of a smart pointer, which firmly keeps the object alive for the duration of the function and the call tree.

##### Example

Consider this code:

    // global (static or heap), or aliased local ...
    shared_ptr<widget> g_p = ...;

    void f(widget& w)
    {
        g();
        use(w);  // A
    }

    void g()
    {
        g_p = ...; // oops, if this was the last shared_ptr to that widget, destroys the widget
    }

The following should not pass code review:

    void my_code()
    {
        // BAD: passing pointer or reference obtained from a nonlocal smart pointer
        //      that could be inadvertently reset somewhere inside f or it callees
        f(*g_p);

        // BAD: same reason, just passing it as a "this" pointer
        g_p->func();
    }

The fix is simple -- take a local copy of the pointer to "keep a ref count" for your call tree:

    void my_code()
    {
        // cheap: 1 increment covers this entire function and all the call trees below us
        auto pin = g_p;

        // GOOD: passing pointer or reference obtained from a local unaliased smart pointer
        f(*pin);

        // GOOD: same reason
        pin->func();
    }

##### Enforcement

* (Simple) Warn if a pointer or reference obtained from a smart pointer variable (`Unique_ptr` or `Shared_ptr`) that is nonlocal, or that is local but potentially aliased, is used in a function call. If the smart pointer is a `Shared_ptr` then suggest taking a local copy of the smart pointer and obtain a pointer or reference from that instead.

# <a name="S-expr"></a>ES: Expressions and statements

Expressions and statements are the lowest and most direct way of expressing actions and computation. Declarations in local scopes are statements.

For naming, commenting, and indentation rules, see [NL: Naming and layout](#S-naming).

General rules:

* [ES.1: Prefer the standard library to other libraries and to "handcrafted code"](#Res-lib)
* [ES.2: Prefer suitable abstractions to direct use of language features](#Res-abstr)

Declaration rules:

* [ES.5: Keep scopes small](#Res-scope)
* [ES.6: Declare names in for-statement initializers and conditions to limit scope](#Res-cond)
* [ES.7: Keep common and local names short, and keep uncommon and nonlocal names longer](#Res-name-length)
* [ES.8: Avoid similar-looking names](#Res-name-similar)
* [ES.9: Avoid `ALL_CAPS` names](#Res-not-CAPS)
* [ES.10: Declare one name (only) per declaration](#Res-name-one)
* [ES.11: Use `auto` to avoid redundant repetition of type names](#Res-auto)
* [ES.12: Do not reuse names in nested scopes](#Res-reuse)
* [ES.20: Always initialize an object](#Res-always)
* [ES.21: Don't introduce a variable (or constant) before you need to use it](#Res-introduce)
* [ES.22: Don't declare a variable until you have a value to initialize it with](#Res-init)
* [ES.23: Prefer the `{}`-initializer syntax](#Res-list)
* [ES.24: Use a `unique_ptr<T>` to hold pointers](#Res-unique)
* [ES.25: Declare an object `const` or `constexpr` unless you want to modify its value later on](#Res-const)
* [ES.26: Don't use a variable for two unrelated purposes](#Res-recycle)
* [ES.27: Use `std::array` or `stack_array` for arrays on the stack](#Res-stack)
* [ES.28: Use lambdas for complex initialization, especially of `const` variables](#Res-lambda-init)
* [ES.30: Don't use macros for program text manipulation](#Res-macros)
* [ES.31: Don't use macros for constants or "functions"](#Res-macros2)
* [ES.32: Use `ALL_CAPS` for all macro names](#Res-ALL_CAPS)
* [ES.33: If you must use macros, give them unique names](#Res-MACROS)
* [ES.34: Don't define a (C-style) variadic function](#Res-ellipses)

Expression rules:

* [ES.40: Avoid complicated expressions](#Res-complicated)
* [ES.41: If in doubt about operator precedence, parenthesize](#Res-parens)
* [ES.42: Keep use of pointers simple and straightforward](#Res-ptr)
* [ES.43: Avoid expressions with undefined order of evaluation](#Res-order)
* [ES.44: Don't depend on order of evaluation of function arguments](#Res-order-fct)
* [ES.45: Avoid "magic constants"; use symbolic constants](#Res-magic)
* [ES.46: Avoid narrowing conversions](#Res-narrowing)
* [ES.47: Use `nullptr` rather than `0` or `NULL`](#Res-nullptr)
* [ES.48: Avoid casts](#Res-casts)
* [ES.49: If you must use a cast, use a named cast](#Res-casts-named)
* [ES.50: Don't cast away `const`](#Res-casts-const)
* [ES.55: Avoid the need for range checking](#Res-range-checking)
* [ES.56: Write `std::move()` only when you need to explicitly move an object to another scope](#Res-move)
* [ES.60: Avoid `new` and `delete` outside resource management functions](#Res-new)
* [ES.61: Delete arrays using `delete[]` and non-arrays using `delete`](#Res-del)
* [ES.62: Don't compare pointers into different arrays](#Res-arr2)
* [ES.63: Don't slice](#Res-slice)
* [ES.64: Use the `T{e}`notation for construction](#Res-construct)
* [ES.65: Don't dereference an invalid pointer](#Res-deref)

Statement rules:

* [ES.70: Prefer a `switch`-statement to an `if`-statement when there is a choice](#Res-switch-if)
* [ES.71: Prefer a range-`for`-statement to a `for`-statement when there is a choice](#Res-for-range)
* [ES.72: Prefer a `for`-statement to a `while`-statement when there is an obvious loop variable](#Res-for-while)
* [ES.73: Prefer a `while`-statement to a `for`-statement when there is no obvious loop variable](#Res-while-for)
* [ES.74: Prefer to declare a loop variable in the initializer part of a `for`-statement](#Res-for-init)
* [ES.75: Avoid `do`-statements](#Res-do)
* [ES.76: Avoid `goto`](#Res-goto)
* [ES.77: Minimize the use of `break` and `continue` in loops](#Res-continue)
* [ES.78: Always end a non-empty `case` with a `break`](#Res-break)
* [ES.79: Use `default` to handle common cases (only)](#Res-default)
* [ES.84: Don't try to declare a local variable with no name](#Res-noname)
* [ES.85: Make empty statements visible](#Res-empty)
* [ES.86: Avoid modifying loop control variables inside the body of raw for-loops](#Res-loop-counter)
* [ES.87: Don't add redundant `==` or `!=` to conditions](#Res-if)

Arithmetic rules:

* [ES.100: Don't mix signed and unsigned arithmetic](#Res-mix)
* [ES.101: Use unsigned types for bit manipulation](#Res-unsigned)
* [ES.102: Use signed types for arithmetic](#Res-signed)
* [ES.103: Don't overflow](#Res-overflow)
* [ES.104: Don't underflow](#Res-underflow)
* [ES.105: Don't divide by zero](#Res-zero)
* [ES.106: Don't try to avoid negative values by using `unsigned`](#Res-nonnegative)
* [ES.107: Don't use `unsigned` for subscripts, prefer `gsl::index`](#Res-subscripts)

### <a name="Res-lib"></a>ES.1: Prefer the standard library to other libraries and to "handcrafted code"

##### Reason

Code using a library can be much easier to write than code working directly with language features, much shorter, tend to be of a higher level of abstraction, and the library code is presumably already tested.
The ISO C++ Standard Library is among the most widely known and best tested libraries.
It is available as part of all C++ Implementations.

##### Example

    auto sum = accumulate(begin(a), end(a), 0.0);   // good

a range version of `accumulate` would be even better:

    auto sum = accumulate(v, 0.0); // better

but don't hand-code a well-known algorithm:

    int max = v.size();   // bad: verbose, purpose unstated
    double sum = 0.0;
    for (int i = 0; i < max; ++i)
        sum = sum + v[i];

##### Exception

Large parts of the standard library rely on dynamic allocation (free store). These parts, notably the containers but not the algorithms, are unsuitable for some hard-real-time and embedded applications. In such cases, consider providing/using similar facilities, e.g.,  a standard-library-style container implemented using a pool allocator.

##### Enforcement

Not easy. ??? Look for messy loops, nested loops, long functions, absence of function calls, lack of use of non-built-in types. Cyclomatic complexity?

### <a name="Res-abstr"></a>ES.2: Prefer suitable abstractions to direct use of language features

##### Reason

A "suitable abstraction" (e.g., library or class) is closer to the application concepts than the bare language, leads to shorter and clearer code, and is likely to be better tested.

##### Example

    vector<string> read1(istream& is)   // good
    {
        vector<string> res;
        for (string s; is >> s;)
            res.push_back(s);
        return res;
    }

The more traditional and lower-level near-equivalent is longer, messier, harder to get right, and most likely slower:

    char** read2(istream& is, int maxelem, int maxstring, int* nread)   // bad: verbose and incomplete
    {
        auto res = new char*[maxelem];
        int elemcount = 0;
        while (is && elemcount < maxelem) {
            auto s = new char[maxstring];
            is.read(s, maxstring);
            res[elemcount++] = s;
        }
        nread = &elemcount;
        return res;
    }

Once the checking for overflow and error handling has been added that code gets quite messy, and there is the problem remembering to `delete` the returned pointer and the C-style strings that array contains.

##### Enforcement

Not easy. ??? Look for messy loops, nested loops, long functions, absence of function calls, lack of use of non-built-in types. Cyclomatic complexity?

## ES.dcl: Declarations

A declaration is a statement. A declaration introduces a name into a scope and may cause the construction of a named object.

### <a name="Res-scope"></a>ES.5: Keep scopes small

##### Reason

Readability. Minimize resource retention. Avoid accidental misuse of value.

**Alternative formulation**: Don't declare a name in an unnecessarily large scope.

##### Example

    void use()
    {
        int i;    // bad: i is needlessly accessible after loop
        for (i = 0; i < 20; ++i) { /* ... */ }
        // no intended use of i here
        for (int i = 0; i < 20; ++i) { /* ... */ }  // good: i is local to for-loop

        if (auto pc = dynamic_cast<Circle*>(ps)) {  // good: pc is local to if-statement
            // ... deal with Circle ...
        }
        else {
            // ... handle error ...
        }
    }

##### Example, bad

    void use(const string& name)
    {
        string fn = name + ".txt";
        ifstream is {fn};
        Record r;
        is >> r;
        // ... 200 lines of code without intended use of fn or is ...
    }

This function is by most measure too long anyway, but the point is that the resources used by `fn` and the file handle held by `is`
are retained for much longer than needed and that unanticipated use of `is` and `fn` could happen later in the function.
In this case, it might be a good idea to factor out the read:

    Record load_record(const string& name)
    {
        string fn = name + ".txt";
        ifstream is {fn};
        Record r;
        is >> r;
        return r;
    }

    void use(const string& name)
    {
        Record r = load_record(name);
        // ... 200 lines of code ...
    }

##### Enforcement

* Flag loop variable declared outside a loop and not used after the loop
* Flag when expensive resources, such as file handles and locks are not used for N-lines (for some suitable N)

### <a name="Res-cond"></a>ES.6: Declare names in for-statement initializers and conditions to limit scope

##### Reason

Readability. Minimize resource retention.

##### Example

    void use()
    {
        for (string s; cin >> s;)
            v.push_back(s);

        for (int i = 0; i < 20; ++i) {   // good: i is local to for-loop
            // ...
        }

        if (auto pc = dynamic_cast<Circle*>(ps)) {   // good: pc is local to if-statement
            // ... deal with Circle ...
        }
        else {
            // ... handle error ...
        }
    }

##### Enforcement

* Flag loop variables declared before the loop and not used after the loop
* (hard) Flag loop variables declared before the loop and used after the loop for an unrelated purpose.

##### C++17 and C++20 example

Note: C++17 and C++20 also add `if`, `switch`, and range-`for` initializer statements. These require C++17 and C++20 support.

    map<int, string> mymap;

    if (auto result = mymap.insert(value); result.second) {
        // insert succeeded, and result is valid for this block
        use(result.first);  // ok
        // ...
    } // result is destroyed here

##### C++17 and C++20 enforcement (if using a C++17 or C++20 compiler)

* Flag selection/loop variables declared before the body and not used after the body
* (hard) Flag selection/loop variables declared before the body and used after the body for an unrelated purpose.



### <a name="Res-name-length"></a>ES.7: Keep common and local names short, and keep uncommon and nonlocal names longer

##### Reason

Readability. Lowering the chance of clashes between unrelated non-local names.

##### Example

Conventional short, local names increase readability:

    template<typename T>    // good
    void print(ostream& os, const vector<T>& v)
    {
        for (gsl::index i = 0; i < v.size(); ++i)
            os << v[i] << '\n';
    }

An index is conventionally called `i` and there is no hint about the meaning of the vector in this generic function, so `v` is as good name as any. Compare

    template<typename Element_type>   // bad: verbose, hard to read
    void print(ostream& target_stream, const vector<Element_type>& current_vector)
    {
        for (gsl::index current_element_index = 0;
             current_element_index < current_vector.size();
             ++current_element_index
        )
        target_stream << current_vector[current_element_index] << '\n';
    }

Yes, it is a caricature, but we have seen worse.

##### Example

Unconventional and short non-local names obscure code:

    void use1(const string& s)
    {
        // ...
        tt(s);   // bad: what is tt()?
        // ...
    }

Better, give non-local entities readable names:

    void use1(const string& s)
    {
        // ...
        trim_tail(s);   // better
        // ...
    }

Here, there is a chance that the reader knows what `trim_tail` means and that the reader can remember it after looking it up.

##### Example, bad

Argument names of large functions are de facto non-local and should be meaningful:

    void complicated_algorithm(vector<Record>& vr, const vector<int>& vi, map<string, int>& out)
    // read from events in vr (marking used Records) for the indices in
    // vi placing (name, index) pairs into out
    {
        // ... 500 lines of code using vr, vi, and out ...
    }

We recommend keeping functions short, but that rule isn't universally adhered to and naming should reflect that.

##### Enforcement

Check length of local and non-local names. Also take function length into account.

### <a name="Res-name-similar"></a>ES.8: Avoid similar-looking names

##### Reason

Code clarity and readability. Too-similar names slow down comprehension and increase the likelihood of error.

##### Example, bad

    if (readable(i1 + l1 + ol + o1 + o0 + ol + o1 + I0 + l0)) surprise();

##### Example, bad

Do not declare a non-type with the same name as a type in the same scope. This removes the need to disambiguate with a keyword such as `struct` or `enum`. It also removes a source of errors, as `struct X` can implicitly declare `X` if lookup fails.

    struct foo { int n; };
    struct foo foo();       // BAD, foo is a type already in scope
    struct foo x = foo();   // requires disambiguation

##### Exception

Antique header files might declare non-types and types with the same name in the same scope.

##### Enforcement

* Check names against a list of known confusing letter and digit combinations.
* Flag a declaration of a variable, function, or enumerator that hides a class or enumeration declared in the same scope.

### <a name="Res-not-CAPS"></a>ES.9: Avoid `ALL_CAPS` names

##### Reason

Such names are commonly used for macros. Thus, `ALL_CAPS` name are vulnerable to unintended macro substitution.

##### Example

    // somewhere in some header:
    #define NE !=

    // somewhere else in some other header:
    enum Coord { N, NE, NW, S, SE, SW, E, W };

    // somewhere third in some poor programmer's .cpp:
    switch (direction) {
    case N:
        // ...
    case NE:
        // ...
    // ...
    }

##### Note

Do not use `ALL_CAPS` for constants just because constants used to be macros.

##### Enforcement

Flag all uses of ALL CAPS. For older code, accept ALL CAPS for macro names and flag all non-ALL-CAPS macro names.

### <a name="Res-name-one"></a>ES.10: Declare one name (only) per declaration

##### Reason

One declaration per line increases readability and avoids mistakes related to
the C/C++ grammar. It also leaves room for a more descriptive end-of-line
comment.

##### Example, bad

    char *p, c, a[7], *pp[7], **aa[10];   // yuck!

##### Exception

A function declaration can contain several function argument declarations.

##### Exception

A structured binding (C++17) is specifically designed to introduce several variables:

    auto [iter, inserted] = m.insert_or_assign(k, val);
    if (inserted) { /* new entry was inserted */ }

##### Example

    template <class InputIterator, class Predicate>
    bool any_of(InputIterator first, InputIterator last, Predicate pred);

or better using concepts:

    bool any_of(InputIterator first, InputIterator last, Predicate pred);

##### Example

    double scalbn(double x, int n);   // OK: x * pow(FLT_RADIX, n); FLT_RADIX is usually 2

or:

    double scalbn(    // better: x * pow(FLT_RADIX, n); FLT_RADIX is usually 2
        double x,     // base value
        int n         // exponent
    );

or:

    // better: base * pow(FLT_RADIX, exponent); FLT_RADIX is usually 2
    double scalbn(double base, int exponent);

##### Example

    int a = 7, b = 9, c, d = 10, e = 3;

In a long list of declarators it is easy to overlook an uninitialized variable.

##### Enforcement

Flag variable and constant declarations with multiple declarators (e.g., `int* p, q;`)

### <a name="Res-auto"></a>ES.11: Use `auto` to avoid redundant repetition of type names

##### Reason

* Simple repetition is tedious and error-prone.
* When you use `auto`, the name of the declared entity is in a fixed position in the declaration, increasing readability.
* In a template function declaration the return type can be a member type.

##### Example

Consider:

    auto p = v.begin();   // vector<int>::iterator
    auto h = t.future();
    auto q = make_unique<int[]>(s);
    auto f = [](int x){ return x + 10; };

In each case, we save writing a longish, hard-to-remember type that the compiler already knows but a programmer could get wrong.

##### Example

    template<class T>
    auto Container<T>::first() -> Iterator;   // Container<T>::Iterator

##### Exception

Avoid `auto` for initializer lists and in cases where you know exactly which type you want and where an initializer might require conversion.

##### Example

    auto lst = { 1, 2, 3 };   // lst is an initializer list
    auto x{1};   // x is an int (in C++17; initializer_list in C++11)

##### Note

When concepts become available, we can (and should) be more specific about the type we are deducing:

    // ...
    ForwardIterator p = algo(x, y, z);

##### Example (C++17)

    auto [ quotient, remainder ] = div(123456, 73);   // break out the members of the div_t result

##### Enforcement

Flag redundant repetition of type names in a declaration.

### <a name="Res-reuse"></a>ES.12: Do not reuse names in nested scopes

##### Reason

It is easy to get confused about which variable is used.
Can cause maintenance problems.

##### Example, bad

    int d = 0;
    // ...
    if (cond) {
        // ...
        d = 9;
        // ...
    }
    else {
        // ...
        int d = 7;
        // ...
        d = value_to_be_returned;
        // ...
    }

    return d;

If this is a large `if`-statement, it is easy to overlook that a new `d` has been introduced in the inner scope.
This is a known source of bugs.
Sometimes such reuse of a name in an inner scope is called "shadowing".

##### Note

Shadowing is primarily a problem when functions are too large and too complex.

##### Example

Shadowing of function arguments in the outermost block is disallowed by the language:

    void f(int x)
    {
        int x = 4;  // error: reuse of function argument name

        if (x) {
            int x = 7;  // allowed, but bad
            // ...
        }
    }

##### Example, bad

Reuse of a member name as a local variable can also be a problem:

    struct S {
        int m;
        void f(int x);
    };

    void S::f(int x)
    {
        m = 7;    // assign to member
        if (x) {
            int m = 9;
            // ...
            m = 99; // assign to local variable
            // ...
        }
    }

##### Exception

We often reuse function names from a base class in a derived class:

    struct B {
        void f(int);
    };

    struct D : B {
        void f(double);
        using B::f;
    };

This is error-prone.
For example, had we forgotten the using declaration, a call `d.f(1)` would not have found the `int` version of `f`.

??? Do we need a specific rule about shadowing/hiding in class hierarchies?

##### Enforcement

* Flag reuse of a name in nested local scopes
* Flag reuse of a member name as a local variable in a member function
* Flag reuse of a global name as a local variable or a member name
* Flag reuse of a base class member name in a derived class (except for function names)

### <a name="Res-always"></a>ES.20: Always initialize an object

##### Reason

Avoid used-before-set errors and their associated undefined behavior.
Avoid problems with comprehension of complex initialization.
Simplify refactoring.

##### Example

    void use(int arg)
    {
        int i;   // bad: uninitialized variable
        // ...
        i = 7;   // initialize i
    }

No, `i = 7` does not initialize `i`; it assigns to it. Also, `i` can be read in the `...` part. Better:

    void use(int arg)   // OK
    {
        int i = 7;   // OK: initialized
        string s;    // OK: default initialized
        // ...
    }

##### Note

The *always initialize* rule is deliberately stronger than the *an object must be set before used* language rule.
The latter, more relaxed rule, catches the technical bugs, but:

* It leads to less readable code
* It encourages people to declare names in greater than necessary scopes
* It leads to harder to read code
* It leads to logic bugs by encouraging complex code
* It hampers refactoring

The *always initialize* rule is a style rule aimed to improve maintainability as well as a rule protecting against used-before-set errors.

##### Example

Here is an example that is often considered to demonstrate the need for a more relaxed rule for initialization

    widget i;    // "widget" a type that's expensive to initialize, possibly a large POD
    widget j;

    if (cond) {  // bad: i and j are initialized "late"
        i = f1();
        j = f2();
    }
    else {
        i = f3();
        j = f4();
    }

This cannot trivially be rewritten to initialize `i` and `j` with initializers.
Note that for types with a default constructor, attempting to postpone initialization simply leads to a default initialization followed by an assignment.
A popular reason for such examples is "efficiency", but a compiler that can detect whether we made a used-before-set error can also eliminate any redundant double initialization.

Assuming that there is a logical connection between `i` and `j`, that connection should probably be expressed in code:

    pair<widget, widget> make_related_widgets(bool x)
    {
        return (x) ? {f1(), f2()} : {f3(), f4() };
    }

    auto [i, j] = make_related_widgets(cond);    // C++17

##### Note

Complex initialization has been popular with clever programmers for decades.
It has also been a major source of errors and complexity.
Many such errors are introduced during maintenance years after the initial implementation.

##### Example

This rule covers member variables.

    class X {
    public:
        X(int i, int ci) : m2{i}, cm2{ci} {}
        // ...

    private:
        int m1 = 7;
        int m2;
        int m3;

        const int cm1 = 7;
        const int cm2;
        const int cm3;
    };

The compiler will flag the uninitialized `cm3` because it is a `const`, but it will not catch the lack of initialization of `m3`.
Usually, a rare spurious member initialization is worth the absence of errors from lack of initialization and often an optimizer
can eliminate a redundant initialization (e.g., an initialization that occurs immediately before an assignment).

##### Exception

If you are declaring an object that is just about to be initialized from input, initializing it would cause a double initialization.
However, beware that this may leave uninitialized data beyond the input -- and that has been a fertile source of errors and security breaches:

    constexpr int max = 8 * 1024;
    int buf[max];         // OK, but suspicious: uninitialized
    f.read(buf, max);

The cost of initializing that array could be significant in some situations.
However, such examples do tend to leave uninitialized variables accessible, so they should be treated with suspicion.

    constexpr int max = 8 * 1024;
    int buf[max] = {};   // zero all elements; better in some situations
    f.read(buf, max);

When feasible use a library function that is known not to overflow. For example:

    string s;   // s is default initialized to ""
    cin >> s;   // s expands to hold the string

Don't consider simple variables that are targets for input operations exceptions to this rule:

    int i;   // bad
    // ...
    cin >> i;

In the not uncommon case where the input target and the input operation get separated (as they should not) the possibility of used-before-set opens up.

    int i2 = 0;   // better, assuming that zero is an acceptable value for i2
    // ...
    cin >> i2;

A good optimizer should know about input operations and eliminate the redundant operation.

##### Example

Using a value representing "uninitialized" is a symptom of a problem and not a solution:

    widget i = uninit;  // bad
    widget j = uninit;

    // ...
    use(i);         // possibly used before set
    // ...

    if (cond) {     // bad: i and j are initialized "late"
        i = f1();
        j = f2();
    }
    else {
        i = f3();
        j = f4();
    }

Now the compiler cannot even simply detect a used-before-set. Further, we've introduced complexity in the state space for widget: which operations are valid on an `uninit` widget and which are not?

##### Note

Sometimes, a lambda can be used as an initializer to avoid an uninitialized variable:

    error_code ec;
    Value v = [&] {
        auto p = get_value();   // get_value() returns a pair<error_code, Value>
        ec = p.first;
        return p.second;
    }();

or maybe:

    Value v = [] {
        auto p = get_value();   // get_value() returns a pair<error_code, Value>
        if (p.first) throw Bad_value{p.first};
        return p.second;
    }();

**See also**: [ES.28](#Res-lambda-init)

##### Enforcement

* Flag every uninitialized variable.
  Don't flag variables of user-defined types with default constructors.
* Check that an uninitialized buffer is written into *immediately* after declaration.
  Passing an uninitialized variable as a reference to non-`const` argument can be assumed to be a write into the variable.

### <a name="Res-introduce"></a>ES.21: Don't introduce a variable (or constant) before you need to use it

##### Reason

Readability. To limit the scope in which the variable can be used.

##### Example

    int x = 7;
    // ... no use of x here ...
    ++x;

##### Enforcement

Flag declarations that are distant from their first use.

### <a name="Res-init"></a>ES.22: Don't declare a variable until you have a value to initialize it with

##### Reason

Readability. Limit the scope in which a variable can be used. Don't risk used-before-set. Initialization is often more efficient than assignment.

##### Example, bad

    string s;
    // ... no use of s here ...
    s = "what a waste";

##### Example, bad

    SomeLargeType var;   // ugly CaMeLcAsEvArIaBlE

    if (cond)   // some non-trivial condition
        Set(&var);
    else if (cond2 || !cond3) {
        var = Set2(3.14);
    }
    else {
        var = 0;
        for (auto& e : something)
            var += e;
    }

    // use var; that this isn't done too early can be enforced statically with only control flow

This would be fine if there was a default initialization for `SomeLargeType` that wasn't too expensive.
Otherwise, a programmer might very well wonder if every possible path through the maze of conditions has been covered.
If not, we have a "use before set" bug. This is a maintenance trap.

For initializers of moderate complexity, including for `const` variables, consider using a lambda to express the initializer; see [ES.28](#Res-lambda-init).

##### Enforcement

* Flag declarations with default initialization that are assigned to before they are first read.
* Flag any complicated computation after an uninitialized variable and before its use.

### <a name="Res-list"></a>ES.23: Prefer the `{}` initializer syntax

##### Reason

The rules for `{}` initialization are simpler, more general, less ambiguous, and safer than for other forms of initialization.

##### Example

    int x {f(99)};
    vector<int> v = {1, 2, 3, 4, 5, 6};

##### Exception

For containers, there is a tradition for using `{...}` for a list of elements and `(...)` for sizes:

    vector<int> v1(10);    // vector of 10 elements with the default value 0
    vector<int> v2 {10};   // vector of 1 element with the value 10

##### Note

`{}`-initializers do not allow narrowing conversions (and that is usually a good thing).

##### Example

    int x {7.9};   // error: narrowing
    int y = 7.9;   // OK: y becomes 7. Hope for a compiler warning
    int z = gsl::narrow_cast<int>(7.9);  // OK: you asked for it

##### Note

`{}` initialization can be used for all initialization; other forms of initialization can't:

    auto p = new vector<int> {1, 2, 3, 4, 5};   // initialized vector
    D::D(int a, int b) :m{a, b} {   // member initializer (e.g., m might be a pair)
        // ...
    };
    X var {};   // initialize var to be empty
    struct S {
        int m {7};   // default initializer for a member
        // ...
    };

For that reason, `{}`-initialization is often called "uniform initialization"
(though there unfortunately are a few irregularities left).

##### Note

Initialization of a variable declared using `auto` with a single value, e.g., `{v}`, had surprising results until C++17.
The C++17 rules are somewhat less surprising:

    auto x1 {7};        // x1 is an int with the value 7
    auto x2 = {7};  // x2 is an initializer_list<int> with an element 7

    auto x11 {7, 8};    // error: two initializers
    auto x22 = {7, 8};  // x22 is an initializer_list<int> with elements 7 and 8

Use `={...}` if you really want an `initializer_list<T>`

    auto fib10 = {1, 1, 2, 3, 5, 8, 13, 21, 34, 55};   // fib10 is a list

##### Note

`={}` gives copy initialization whereas `{}` gives direct initialization.
Like the distinction between copy-initialization and direct-initialization itself, this can lead to surprises.
`{}` accepts `explicit` constructors; `={}` does not`. For example:

    struct Z { explicit Z() {} };

    Z z1{};     // OK: direct initialization, so we use explicit constructor
    Z z2 = {};  // error: copy initialization, so we cannot use the explicit constructor

Use plain `{}`-initialization unless you specifically want to disable explicit constructors.

##### Note

Old habits die hard, so this rule is hard to apply consistently, especially as there are so many cases where `=` is innocent.

##### Example

    template<typename T>
    void f()
    {
        T x1(1);    // T initialized with 1
        T x0();     // bad: function declaration (often a mistake)

        T y1 {1};   // T initialized with 1
        T y0 {};    // default initialized T
        // ...
    }

**See also**: [Discussion](#???)

##### Enforcement

Tricky.

* Don't flag uses of `=` for simple initializers.
* Look for `=` after `auto` has been seen.

### <a name="Res-unique"></a>ES.24: Use a `unique_ptr<T>` to hold pointers

##### Reason

Using `std::unique_ptr` is the simplest way to avoid leaks. It is reliable, it
makes the type system do much of the work to validate ownership safety, it
increases readability, and it has zero or near zero run-time cost.

##### Example

    void use(bool leak)
    {
        auto p1 = make_unique<int>(7);   // OK
        int* p2 = new int{7};            // bad: might leak
        // ... no assignment to p2 ...
        if (leak) return;
        // ... no assignment to p2 ...
        vector<int> v(7);
        v.at(7) = 0;                    // exception thrown
        // ...
    }

If `leak == true` the object pointed to by `p2` is leaked and the object pointed to by `p1` is not.
The same is the case when `at()` throws.

##### Enforcement

Look for raw pointers that are targets of `new`, `malloc()`, or functions that may return such pointers.

### <a name="Res-const"></a>ES.25: Declare an object `const` or `constexpr` unless you want to modify its value later on

##### Reason

That way you can't change the value by mistake. That way may offer the compiler optimization opportunities.

##### Example

    void f(int n)
    {
        const int bufmax = 2 * n + 2;  // good: we can't change bufmax by accident
        int xmax = n;                  // suspicious: is xmax intended to change?
        // ...
    }

##### Enforcement

Look to see if a variable is actually mutated, and flag it if
not. Unfortunately, it may be impossible to detect when a non-`const` was not
*intended* to vary (vs when it merely did not vary).

### <a name="Res-recycle"></a>ES.26: Don't use a variable for two unrelated purposes

##### Reason

Readability and safety.

##### Example, bad

    void use()
    {
        int i;
        for (i = 0; i < 20; ++i) { /* ... */ }
        for (i = 0; i < 200; ++i) { /* ... */ } // bad: i recycled
    }

##### Note

As an optimization, you may want to reuse a buffer as a scratch pad, but even then prefer to limit the variable's scope as much as possible and be careful not to cause bugs from data left in a recycled buffer as this is a common source of security bugs.

    void write_to_file() {
        std::string buffer;             // to avoid reallocations on every loop iteration
        for (auto& o : objects)
        {
            // First part of the work.
            generate_first_String(buffer, o);
            write_to_file(buffer);

            // Second part of the work.
            generate_second_string(buffer, o);
            write_to_file(buffer);

            // etc...
        }
    }

##### Enforcement

Flag recycled variables.

### <a name="Res-stack"></a>ES.27: Use `std::array` or `stack_array` for arrays on the stack

##### Reason

They are readable and don't implicitly convert to pointers.
They are not confused with non-standard extensions of built-in arrays.

##### Example, bad

    const int n = 7;
    int m = 9;

    void f()
    {
        int a1[n];
        int a2[m];   // error: not ISO C++
        // ...
    }

##### Note

The definition of `a1` is legal C++ and has always been.
There is a lot of such code.
It is error-prone, though, especially when the bound is non-local.
Also, it is a "popular" source of errors (buffer overflow, pointers from array decay, etc.).
The definition of `a2` is C but not C++ and is considered a security risk

##### Example

    const int n = 7;
    int m = 9;

    void f()
    {
        array<int, n> a1;
        stack_array<int> a2(m);
        // ...
    }

##### Enforcement

* Flag arrays with non-constant bounds (C-style VLAs)
* Flag arrays with non-local constant bounds

### <a name="Res-lambda-init"></a>ES.28: Use lambdas for complex initialization, especially of `const` variables

##### Reason

It nicely encapsulates local initialization, including cleaning up scratch variables needed only for the initialization, without needing to create a needless nonlocal yet nonreusable function. It also works for variables that should be `const` but only after some initialization work.

##### Example, bad

    widget x;   // should be const, but:
    for (auto i = 2; i <= N; ++i) {          // this could be some
        x += some_obj.do_something_with(i);  // arbitrarily long code
    }                                        // needed to initialize x
    // from here, x should be const, but we can't say so in code in this style

##### Example, good

    const widget x = [&]{
        widget val;                                // assume that widget has a default constructor
        for (auto i = 2; i <= N; ++i) {            // this could be some
            val += some_obj.do_something_with(i);  // arbitrarily long code
        }                                          // needed to initialize x
        return val;
    }();

##### Example

    string var = [&]{
        if (!in) return "";   // default
        string s;
        for (char c : in >> c)
            s += toupper(c);
        return s;
    }(); // note ()

If at all possible, reduce the conditions to a simple set of alternatives (e.g., an `enum`) and don't mix up selection and initialization.

##### Enforcement

Hard. At best a heuristic. Look for an uninitialized variable followed by a loop assigning to it.

### <a name="Res-macros"></a>ES.30: Don't use macros for program text manipulation

##### Reason

Macros are a major source of bugs.
Macros don't obey the usual scope and type rules.
Macros ensure that the human reader sees something different from what the compiler sees.
Macros complicate tool building.

##### Example, bad

    #define Case break; case   /* BAD */

This innocuous-looking macro makes a single lower case `c` instead of a `C` into a bad flow-control bug.

##### Note

This rule does not ban the use of macros for "configuration control" use in `#ifdef`s, etc.

In the future, modules are likely to eliminate the need for macros in configuration control.

##### Note

This rule is meant to also discourage use of `#` for stringification and `##` for concatenation.
As usual for macros, there are uses that are "mostly harmless", but even these can create problems for tools,
such as auto completers, static analyzers, and debuggers.
Often the desire to use fancy macros is a sign of an overly complex design.
Also, `#` and `##` encourages the definition and use of macros:

    #define CAT(a, b) a ## b
    #define STRINGIFY(a) #a

    void f(int x, int y)
    {
        string CAT(x, y) = "asdf";   // BAD: hard for tools to handle (and ugly)
        string sx2 = STRINGIFY(x);
        // ...
    }

There are workarounds for low-level string manipulation using macros. For example:

    string s = "asdf" "lkjh";   // ordinary string literal concatenation

    enum E { a, b };

    template<int x>
    constexpr const char* stringify()
    {
        switch (x) {
        case a: return "a";
        case b: return "b";
        }
    }

    void f(int x, int y)
    {
        string sx = stringify<x>();
        // ...
    }

This is not as convenient as a macro to define, but as easy to use, has zero overhead, and is typed and scoped.

In the future, static reflection is likely to eliminate the last needs for the preprocessor for program text manipulation.

##### Enforcement

Scream when you see a macro that isn't just used for source control (e.g., `#ifdef`)

### <a name="Res-macros2"></a>ES.31: Don't use macros for constants or "functions"

##### Reason

Macros are a major source of bugs.
Macros don't obey the usual scope and type rules.
Macros don't obey the usual rules for argument passing.
Macros ensure that the human reader sees something different from what the compiler sees.
Macros complicate tool building.

##### Example, bad

    #define PI 3.14
    #define SQUARE(a, b) (a * b)

Even if we hadn't left a well-known bug in `SQUARE` there are much better behaved alternatives; for example:

    constexpr double pi = 3.14;
    template<typename T> T square(T a, T b) { return a * b; }

##### Enforcement

Scream when you see a macro that isn't just used for source control (e.g., `#ifdef`)

### <a name="Res-ALL_CAPS"></a>ES.32: Use `ALL_CAPS` for all macro names

##### Reason

Convention. Readability. Distinguishing macros.

##### Example

    #define forever for (;;)   /* very BAD */

    #define FOREVER for (;;)   /* Still evil, but at least visible to humans */

##### Enforcement

Scream when you see a lower case macro.

### <a name="Res-MACROS"></a>ES.33: If you must use macros, give them unique names

##### Reason

Macros do not obey scope rules.

##### Example

    #define MYCHAR        /* BAD, will eventually clash with someone else's MYCHAR*/

    #define ZCORP_CHAR    /* Still evil, but less likely to clash */

##### Note

Avoid macros if you can: [ES.30](#Res-macros), [ES.31](#Res-macros2), and [ES.32](#Res-ALL_CAPS).
However, there are billions of lines of code littered with macros and a long tradition for using and overusing macros.
If you are forced to use macros, use long names and supposedly unique prefixes (e.g., your organization's name) to lower the likelihood of a clash.

##### Enforcement

Warn against short macro names.

### <a name="Res-ellipses"></a> ES.34: Don't define a (C-style) variadic function

##### Reason

Not type safe.
Requires messy cast-and-macro-laden code to get working right.

##### Example

    #include <cstdarg>

    // "severity" followed by a zero-terminated list of char*s; write the C-style strings to cerr
    void error(int severity ...)
    {
        va_list ap;             // a magic type for holding arguments
        va_start(ap, severity); // arg startup: "severity" is the first argument of error()

        for (;;) {
            // treat the next var as a char*; no checking: a cast in disguise
            char* p = va_arg(ap, char*);
            if (!p) break;
            cerr << p << ' ';
        }

        va_end(ap);             // arg cleanup (don't forget this)

        cerr << '\n';
        if (severity) exit(severity);
    }

    void use()
    {
        error(7, "this", "is", "an", "error", nullptr);
        error(7); // crash
        error(7, "this", "is", "an", "error");  // crash
        const char* is = "is";
        string an = "an";
        error(7, "this", "is", an, "error"); // crash
    }

**Alternative**: Overloading. Templates. Variadic templates.
    #include <iostream>

    void error(int severity)
    {
        std::cerr << '\n';
        std::exit(severity);
    }

    template <typename T, typename... Ts>
    constexpr void error(int severity, T head, Ts... tail)
    {
        std::cerr << head;
        error(severity, tail...);
    }

    void use()
    {
        error(7); // No crash!
        error(5, "this", "is", "not", "an", "error"); // No crash!

        std::string an = "an";
        error(7, "this", "is", "not", an, "error"); // No crash!

        error(5, "oh", "no", nullptr); // Compile error! No need for nullptr.
    }


##### Note

This is basically the way `printf` is implemented.

##### Enforcement

* Flag definitions of C-style variadic functions.
* Flag `#include <cstdarg>` and `#include <stdarg.h>`


## ES.expr: Expressions

Expressions manipulate values.

### <a name="Res-complicated"></a>ES.40: Avoid complicated expressions

##### Reason

Complicated expressions are error-prone.

##### Example

    // bad: assignment hidden in subexpression
    while ((c = getc()) != -1)

    // bad: two non-local variables assigned in sub-expressions
    while ((cin >> c1, cin >> c2), c1 == c2)

    // better, but possibly still too complicated
    for (char c1, c2; cin >> c1 >> c2 && c1 == c2;)

    // OK: if i and j are not aliased
    int x = ++i + ++j;

    // OK: if i != j and i != k
    v[i] = v[j] + v[k];

    // bad: multiple assignments "hidden" in subexpressions
    x = a + (b = f()) + (c = g()) * 7;

    // bad: relies on commonly misunderstood precedence rules
    x = a & b + c * d && e ^ f == 7;

    // bad: undefined behavior
    x = x++ + x++ + ++x;

Some of these expressions are unconditionally bad (e.g., they rely on undefined behavior). Others are simply so complicated and/or unusual that even good programmers could misunderstand them or overlook a problem when in a hurry.

##### Note

C++17 tightens up the rules for the order of evaluation
(left-to-right except right-to-left in assignments, and the order of evaluation of function arguments is unspecified; [see ES.43](#Res-order)),
but that doesn't change the fact that complicated expressions are potentially confusing.

##### Note

A programmer should know and use the basic rules for expressions.

##### Example

    x = k * y + z;             // OK

    auto t1 = k * y;           // bad: unnecessarily verbose
    x = t1 + z;

    if (0 <= x && x < max)   // OK

    auto t1 = 0 <= x;        // bad: unnecessarily verbose
    auto t2 = x < max;
    if (t1 && t2)            // ...

##### Enforcement

Tricky. How complicated must an expression be to be considered complicated? Writing computations as statements with one operation each is also confusing. Things to consider:

* side effects: side effects on multiple non-local variables (for some definition of non-local) can be suspect, especially if the side effects are in separate subexpressions
* writes to aliased variables
* more than N operators (and what should N be?)
* reliance of subtle precedence rules
* uses undefined behavior (can we catch all undefined behavior?)
* implementation defined behavior?
* ???

### <a name="Res-parens"></a>ES.41: If in doubt about operator precedence, parenthesize

##### Reason

Avoid errors. Readability. Not everyone has the operator table memorized.

##### Example

    const unsigned int flag = 2;
    unsigned int a = flag;

    if (a & flag != 0)  // bad: means a&(flag != 0)

Note: We recommend that programmers know their precedence table for the arithmetic operations, the logical operations, but consider mixing bitwise logical operations with other operators in need of parentheses.

    if ((a & flag) != 0)  // OK: works as intended

##### Note

You should know enough not to need parentheses for:

    if (a < 0 || a <= max) {
        // ...
    }

##### Enforcement

* Flag combinations of bitwise-logical operators and other operators.
* Flag assignment operators not as the leftmost operator.
* ???

### <a name="Res-ptr"></a>ES.42: Keep use of pointers simple and straightforward

##### Reason

Complicated pointer manipulation is a major source of errors.

##### Note

Use `gsl::span` instead.
Pointers should [only refer to single objects](#Ri-array).
Pointer arithmetic is fragile and easy to get wrong, the source of many, many bad bugs and security violations.
`span` is a bounds-checked, safe type for accessing arrays of data.
Access into an array with known bounds using a constant as a subscript can be validated by the compiler.

##### Example, bad

    void f(int* p, int count)
    {
        if (count < 2) return;

        int* q = p + 1;    // BAD

        ptrdiff_t d;
        int n;
        d = (p - &n);      // OK
        d = (q - p);       // OK

        int n = *p++;      // BAD

        if (count < 6) return;

        p[4] = 1;          // BAD

        p[count - 1] = 2;  // BAD

        use(&p[0], 3);     // BAD
    }

##### Example, good

    void f(span<int> a) // BETTER: use span in the function declaration
    {
        if (a.size() < 2) return;

        int n = a[0];      // OK

        span<int> q = a.subspan(1); // OK

        if (a.size() < 6) return;

        a[4] = 1;          // OK

        a[a.size() - 1] = 2;  // OK

        use(a.data(), 3);  // OK
    }

##### Note

Subscripting with a variable is difficult for both tools and humans to validate as safe.
`span` is a run-time bounds-checked, safe type for accessing arrays of data.
`at()` is another alternative that ensures single accesses are bounds-checked.
If iterators are needed to access an array, use the iterators from a `span` constructed over the array.

##### Example, bad

    void f(array<int, 10> a, int pos)
    {
        a[pos / 2] = 1; // BAD
        a[pos - 1] = 2; // BAD
        a[-1] = 3;    // BAD (but easily caught by tools) -- no replacement, just don't do this
        a[10] = 4;    // BAD (but easily caught by tools) -- no replacement, just don't do this
    }

##### Example, good

Use a `span`:

    void f1(span<int, 10> a, int pos) // A1: Change parameter type to use span
    {
        a[pos / 2] = 1; // OK
        a[pos - 1] = 2; // OK
    }

    void f2(array<int, 10> arr, int pos) // A2: Add local span and use that
    {
        span<int> a = {arr.data(), pos};
        a[pos / 2] = 1; // OK
        a[pos - 1] = 2; // OK
    }

Use `at()`:

    void f3(array<int, 10> a, int pos) // ALTERNATIVE B: Use at() for access
    {
        at(a, pos / 2) = 1; // OK
        at(a, pos - 1) = 2; // OK
    }

##### Example, bad

    void f()
    {
        int arr[COUNT];
        for (int i = 0; i < COUNT; ++i)
            arr[i] = i; // BAD, cannot use non-constant indexer
    }

##### Example, good

Use a `span`:

    void f1()
    {
        int arr[COUNT];
        span<int> av = arr;
        for (int i = 0; i < COUNT; ++i)
            av[i] = i;
    }

Use a `span` and range-`for`:

    void f1a()
    {
         int arr[COUNT];
         span<int, COUNT> av = arr;
         int i = 0;
         for (auto& e : av)
             e = i++;
    }

Use `at()` for access:

    void f2()
    {
        int arr[COUNT];
        for (int i = 0; i < COUNT; ++i)
            at(arr, i) = i;
    }

Use a range-`for`:

    void f3()
    {
        int arr[COUNT];
        for (auto& e : arr)
             e = i++;
    }

##### Note

Tooling can offer rewrites of array accesses that involve dynamic index expressions to use `at()` instead:

    static int a[10];

    void f(int i, int j)
    {
        a[i + j] = 12;      // BAD, could be rewritten as ...
        at(a, i + j) = 12;  // OK -- bounds-checked
    }

##### Example

Turning an array into a pointer (as the language does essentially always) removes opportunities for checking, so avoid it

    void g(int* p);

    void f()
    {
        int a[5];
        g(a);        // BAD: are we trying to pass an array?
        g(&a[0]);    // OK: passing one object
    }

If you want to pass an array, say so:

    void g(int* p, size_t length);  // old (dangerous) code

    void g1(span<int> av); // BETTER: get g() changed.

    void f2()
    {
        int a[5];
        span<int> av = a;

        g(av.data(), av.size());   // OK, if you have no choice
        g1(a);                     // OK -- no decay here, instead use implicit span ctor
    }

##### Enforcement

* Flag any arithmetic operation on an expression of pointer type that results in a value of pointer type.
* Flag any indexing expression on an expression or variable of array type (either static array or `std::array`) where the indexer is not a compile-time constant expression with a value between `0` and the upper bound of the array.
* Flag any expression that would rely on implicit conversion of an array type to a pointer type.

This rule is part of the [bounds-safety profile](#SS-bounds).


### <a name="Res-order"></a>ES.43: Avoid expressions with undefined order of evaluation

##### Reason

You have no idea what such code does. Portability.
Even if it does something sensible for you, it may do something different on another compiler (e.g., the next release of your compiler) or with a different optimizer setting.

##### Note

C++17 tightens up the rules for the order of evaluation:
left-to-right except right-to-left in assignments, and the order of evaluation of function arguments is unspecified.

However, remember that your code may be compiled with a pre-C++17 compiler (e.g., through cut-and-paste) so don't be too clever.

##### Example

    v[i] = ++i;   //  the result is undefined

A good rule of thumb is that you should not read a value twice in an expression where you write to it.

##### Enforcement

Can be detected by a good analyzer.

### <a name="Res-order-fct"></a>ES.44: Don't depend on order of evaluation of function arguments

##### Reason

Because that order is unspecified.

##### Note

C++17 tightens up the rules for the order of evaluation, but the order of evaluation of function arguments is still unspecified.

##### Example

    int i = 0;
    f(++i, ++i);

The call will most likely be `f(0, 1)` or `f(1, 0)`, but you don't know which.
Technically, the behavior is undefined.
In C++17, this code does not have undefined behavior, but it is still not specified which argument is evaluated first.

##### Example

Overloaded operators can lead to order of evaluation problems:

    f1()->m(f2());          // m(f1(), f2())
    cout << f1() << f2();   // operator<<(operator<<(cout, f1()), f2())

In C++17, these examples work as expected (left to right) and assignments are evaluated right to left (just as ='s binding is right-to-left)

    f1() = f2();    // undefined behavior in C++14; in C++17, f2() is evaluated before f1()

##### Enforcement

Can be detected by a good analyzer.

### <a name="Res-magic"></a>ES.45: Avoid "magic constants"; use symbolic constants

##### Reason

Unnamed constants embedded in expressions are easily overlooked and often hard to understand:

##### Example

    for (int m = 1; m <= 12; ++m)   // don't: magic constant 12
        cout << month[m] << '\n';

No, we don't all know that there are 12 months, numbered 1..12, in a year. Better:

    // months are indexed 1..12
    constexpr int first_month = 1;
    constexpr int last_month = 12;

    for (int m = first_month; m <= last_month; ++m)   // better
        cout << month[m] << '\n';

Better still, don't expose constants:

    for (auto m : month)
        cout << m << '\n';

##### Enforcement

Flag literals in code. Give a pass to `0`, `1`, `nullptr`, `\n`, `""`, and others on a positive list.

### <a name="Res-narrowing"></a>ES.46: Avoid lossy (narrowing, truncating) arithmetic conversions

##### Reason

A narrowing conversion destroys information, often unexpectedly so.

##### Example, bad

A key example is basic narrowing:

    double d = 7.9;
    int i = d;    // bad: narrowing: i becomes 7
    i = (int) d;  // bad: we're going to claim this is still not explicit enough

    void f(int x, long y, double d)
    {
        char c1 = x;   // bad: narrowing
        char c2 = y;   // bad: narrowing
        char c3 = d;   // bad: narrowing
    }

##### Note

The guidelines support library offers a `narrow_cast` operation for specifying that narrowing is acceptable and a `narrow` ("narrow if") that throws an exception if a narrowing would throw away information:

    i = narrow_cast<int>(d);   // OK (you asked for it): narrowing: i becomes 7
    i = narrow<int>(d);        // OK: throws narrowing_error

We also include lossy arithmetic casts, such as from a negative floating point type to an unsigned integral type:

    double d = -7.9;
    unsigned u = 0;

    u = d;                          // BAD
    u = narrow_cast<unsigned>(d);   // OK (you asked for it): u becomes 0
    u = narrow<unsigned>(d);        // OK: throws narrowing_error

##### Enforcement

A good analyzer can detect all narrowing conversions. However, flagging all narrowing conversions will lead to a lot of false positives. Suggestions:

* flag all floating-point to integer conversions (maybe only `float`->`char` and `double`->`int`. Here be dragons! we need data)
* flag all `long`->`char` (I suspect `int`->`char` is very common. Here be dragons! we need data)
* consider narrowing conversions for function arguments especially suspect

### <a name="Res-nullptr"></a>ES.47: Use `nullptr` rather than `0` or `NULL`

##### Reason

Readability. Minimize surprises: `nullptr` cannot be confused with an
`int`. `nullptr` also has a well-specified (very restrictive) type, and thus
works in more scenarios where type deduction might do the wrong thing on `NULL`
or `0`.

##### Example

Consider:

    void f(int);
    void f(char*);
    f(0);         // call f(int)
    f(nullptr);   // call f(char*)

##### Enforcement

Flag uses of `0` and `NULL` for pointers. The transformation may be helped by simple program transformation.

### <a name="Res-casts"></a>ES.48: Avoid casts

##### Reason

Casts are a well-known source of errors. Make some optimizations unreliable.

##### Example, bad

    double d = 2;
    auto p = (long*)&d;
    auto q = (long long*)&d;
    cout << d << ' ' << *p << ' ' << *q << '\n';

What would you think this fragment prints? The result is at best implementation defined. I got

    2 0 4611686018427387904

Adding

    *q = 666;
    cout << d << ' ' << *p << ' ' << *q << '\n';

I got

    3.29048e-321 666 666

Surprised? I'm just glad I didn't crash the program.

##### Note

Programmers who write casts typically assume that they know what they are doing,
or that writing a cast makes the program "easier to read".
In fact, they often disable the general rules for using values.
Overload resolution and template instantiation usually pick the right function if there is a right function to pick.
If there is not, maybe there ought to be, rather than applying a local fix (cast).

##### Note

Casts are necessary in a systems programming language.  For example, how else
would we get the address of a device register into a pointer?  However, casts
are seriously overused as well as a major source of errors.

##### Note

If you feel the need for a lot of casts, there may be a fundamental design problem.

##### Exception

Casting to `(void)` is the Standard-sanctioned way to turn off `[[nodiscard]]` warnings. If you are calling a function with a `[[nodiscard]]` return and you deliberately want to discard the result, first think hard about whether that is really a good idea (there is usually a good reason the author of the function or of the return type used `[[nodiscard]]` in the first place), but if you still think it's appropriate and your code reviewer agrees, write `(void)` to turn off the warning.

##### Alternatives

Casts are widely (mis) used. Modern C++ has rules and constructs that eliminate the need for casts in many contexts, such as

* Use templates
* Use `std::variant`
* Rely on the well-defined, safe, implicit conversions between pointer types

##### Enforcement

* Force the elimination of C-style casts, except on a function with a `[[nodiscard]]` return
* Warn if there are many functional style casts (there is an obvious problem in quantifying 'many')
* The [type profile](#Pro-type-reinterpretcast) bans `reinterpret_cast`.
* Warn against [identity casts](#Pro-type-identitycast) between pointer types, where the source and target types are the same (#Pro-type-identitycast)
* Warn if a pointer cast could be [implicit](#Pro-type-implicitpointercast)

### <a name="Res-casts-named"></a>ES.49: If you must use a cast, use a named cast

##### Reason

Readability. Error avoidance.
Named casts are more specific than a C-style or functional cast, allowing the compiler to catch some errors.

The named casts are:

* `static_cast`
* `const_cast`
* `reinterpret_cast`
* `dynamic_cast`
* `std::move`         // `move(x)` is an rvalue reference to `x`
* `std::forward`      // `forward(x)` is an rvalue reference to `x`
* `gsl::narrow_cast`  // `narrow_cast<T>(x)` is `static_cast<T>(x)`
* `gsl::narrow`       // `narrow<T>(x)` is `static_cast<T>(x)` if `static_cast<T>(x) == x` or it throws `narrowing_error`

##### Example

    class B { /* ... */ };
    class D { /* ... */ };

    template<typename D> D* upcast(B* pb)
    {
        D* pd0 = pb;                        // error: no implicit conversion from B* to D*
        D* pd1 = (D*)pb;                    // legal, but what is done?
        D* pd2 = static_cast<D*>(pb);       // error: D is not derived from B
        D* pd3 = reinterpret_cast<D*>(pb);  // OK: on your head be it!
        D* pd4 = dynamic_cast<D*>(pb);      // OK: return nullptr
        // ...
    }

The example was synthesized from real-world bugs where `D` used to be derived from `B`, but someone refactored the hierarchy.
The C-style cast is dangerous because it can do any kind of conversion, depriving us of any protection from mistakes (now or in the future).

##### Note

When converting between types with no information loss (e.g. from `float` to
`double` or `int64` from `int32`), brace initialization may be used instead.

    double d {some_float};
    int64_t i {some_int32};

This makes it clear that the type conversion was intended and also prevents
conversions between types that might result in loss of precision. (It is a
compilation error to try to initialize a `float` from a `double` in this fashion,
for example.)

##### Note

`reinterpret_cast` can be essential, but the essential uses (e.g., turning a machine address into pointer) are not type safe:

    auto p = reinterpret_cast<Device_register>(0x800);  // inherently dangerous


##### Enforcement

* Flag C-style and functional casts.
* The [type profile](#Pro-type-reinterpretcast) bans `reinterpret_cast`.
* The [type profile](#Pro-type-arithmeticcast) warns when using `static_cast` between arithmetic types.

### <a name="Res-casts-const"></a>ES.50: Don't cast away `const`

##### Reason

It makes a lie out of `const`.
If the variable is actually declared `const`, the result of "casting away `const`" is undefined behavior.

##### Example, bad

    void f(const int& x)
    {
        const_cast<int&>(x) = 42;   // BAD
    }

    static int i = 0;
    static const int j = 0;

    f(i); // silent side effect
    f(j); // undefined behavior

##### Example

Sometimes, you may be tempted to resort to `const_cast` to avoid code duplication, such as when two accessor functions that differ only in `const`-ness have similar implementations. For example:

    class Bar;

    class Foo {
    public:
        // BAD, duplicates logic
        Bar& get_bar() {
            /* complex logic around getting a non-const reference to my_bar */
        }

        const Bar& get_bar() const {
            /* same complex logic around getting a const reference to my_bar */
        }
    private:
        Bar my_bar;
    };

Instead, prefer to share implementations. Normally, you can just have the non-`const` function call the `const` function. However, when there is complex logic this can lead to the following pattern that still resorts to a `const_cast`:

    class Foo {
    public:
        // not great, non-const calls const version but resorts to const_cast
        Bar& get_bar() {
            return const_cast<Bar&>(static_cast<const Foo&>(*this).get_bar());
        }
        const Bar& get_bar() const {
            /* the complex logic around getting a const reference to my_bar */
        }
    private:
        Bar my_bar;
    };

Although this pattern is safe when applied correctly, because the caller must have had a non-`const` object to begin with, it's not ideal because the safety is hard to enforce automatically as a checker rule.

Instead, prefer to put the common code in a common helper function -- and make it a template so that it deduces `const`. This doesn't use any `const_cast` at all:

    class Foo {
    public:                         // good
              Bar& get_bar()       { return get_bar_impl(*this); }
        const Bar& get_bar() const { return get_bar_impl(*this); }
    private:
        Bar my_bar;

        template<class T>           // good, deduces whether T is const or non-const
        static auto get_bar_impl(T& t) -> decltype(t.get_bar())
            { /* the complex logic around getting a possibly-const reference to my_bar */ }
    };

##### Exception

You may need to cast away `const` when calling `const`-incorrect functions.
Prefer to wrap such functions in inline `const`-correct wrappers to encapsulate the cast in one place.

##### Example

Sometimes, "cast away `const`" is to allow the updating of some transient information of an otherwise immutable object.
Examples are caching, memoization, and precomputation.
Such examples are often handled as well or better using `mutable` or an indirection than with a `const_cast`.

Consider keeping previously computed results around for a costly operation:

    int compute(int x); // compute a value for x; assume this to be costly

    class Cache {   // some type implementing a cache for an int->int operation
    public:
        pair<bool, int> find(int x) const;   // is there a value for x?
        void set(int x, int v);             // make y the value for x
        // ...
    private:
        // ...
    };

    class X {
    public:
        int get_val(int x)
        {
            auto p = cache.find(x);
            if (p.first) return p.second;
            int val = compute(x);
            cache.set(x, val); // insert value for x
            return val;
        }
        // ...
    private:
        Cache cache;
    };

Here, `get_val()` is logically constant, so we would like to make it a `const` member.
To do this we still need to mutate `cache`, so people sometimes resort to a `const_cast`:

    class X {   // Suspicious solution based on casting
    public:
        int get_val(int x) const
        {
            auto p = cache.find(x);
            if (p.first) return p.second;
            int val = compute(x);
            const_cast<Cache&>(cache).set(x, val);   // ugly
            return val;
        }
        // ...
    private:
        Cache cache;
    };

Fortunately, there is a better solution:
State that `cache` is mutable even for a `const` object:

    class X {   // better solution
    public:
        int get_val(int x) const
        {
            auto p = cache.find(x);
            if (p.first) return p.second;
            int val = compute(x);
            cache.set(x, val);
            return val;
        }
        // ...
    private:
        mutable Cache cache;
    };

An alternative solution would be to store a pointer to the `cache`:

    class X {   // OK, but slightly messier solution
    public:
        int get_val(int x) const
        {
            auto p = cache->find(x);
            if (p.first) return p.second;
            int val = compute(x);
            cache->set(x, val);
            return val;
        }
        // ...
    private:
        unique_ptr<Cache> cache;
    };

That solution is the most flexible, but requires explicit construction and destruction of `*cache`
(most likely in the constructor and destructor of `X`).

In any variant, we must guard against data races on the `cache` in multi-threaded code, possibly using a `std::mutex`.

##### Enforcement

* Flag `const_cast`s.
* This rule is part of the [type-safety profile](#Pro-type-constcast) for the related Profile.

### <a name="Res-range-checking"></a>ES.55: Avoid the need for range checking

##### Reason

Constructs that cannot overflow do not overflow (and usually run faster):

##### Example

    for (auto& x : v)      // print all elements of v
        cout << x << '\n';

    auto p = find(v, x);   // find x in v

##### Enforcement

Look for explicit range checks and heuristically suggest alternatives.

### <a name="Res-move"></a>ES.56: Write `std::move()` only when you need to explicitly move an object to another scope

##### Reason

We move, rather than copy, to avoid duplication and for improved performance.

A move typically leaves behind an empty object ([C.64](#Rc-move-semantic)), which can be surprising or even dangerous, so we try to avoid moving from lvalues (they might be accessed later).

##### Notes

Moving is done implicitly when the source is an rvalue (e.g., value in a `return` treatment or a function result), so don't pointlessly complicate code in those cases by writing `move` explicitly. Instead, write short functions that return values, and both the function's return and the caller's accepting of the return will be optimized naturally.

In general, following the guidelines in this document (including not making variables' scopes needlessly large, writing short functions that return values, returning local variables) help eliminate most need for explicit `std::move`.

Explicit `move` is needed to explicitly move an object to another scope, notably to pass it to a "sink" function and in the implementations of the move operations themselves (move constructor, move assignment operator) and swap operations.

##### Example, bad

    void sink(X&& x);   // sink takes ownership of x

    void user()
    {
        X x;
        // error: cannot bind an lvalue to a rvalue reference
        sink(x);
        // OK: sink takes the contents of x, x must now be assumed to be empty
        sink(std::move(x));

        // ...

        // probably a mistake
        use(x);
    }

Usually, a `std::move()` is used as an argument to a `&&` parameter.
And after you do that, assume the object has been moved from (see [C.64](#Rc-move-semantic)) and don't read its state again until you first set it to a new value.

    void f() {
        string s1 = "supercalifragilisticexpialidocious";

        string s2 = s1;             // ok, takes a copy
        assert(s1 == "supercalifragilisticexpialidocious");  // ok

        // bad, if you want to keep using s1's value
        string s3 = move(s1);

        // bad, assert will likely fail, s1 likely changed
        assert(s1 == "supercalifragilisticexpialidocious");
    }

##### Example

    void sink(unique_ptr<widget> p);  // pass ownership of p to sink()

    void f() {
        auto w = make_unique<widget>();
        // ...
        sink(std::move(w));               // ok, give to sink()
        // ...
        sink(w);    // Error: unique_ptr is carefully designed so that you cannot copy it
    }

##### Notes

`std::move()` is a cast to `&&` in disguise; it doesn't itself move anything, but marks a named object as a candidate that can be moved from.
The language already knows the common cases where objects can be moved from, especially when returning values from functions, so don't complicate code with redundant `std::move()`'s.

Never write `std::move()` just because you've heard "it's more efficient."
In general, don't believe claims of "efficiency" without data (???).
In general, don't complicate your code without reason (??)

##### Example, bad

    vector<int> make_vector() {
        vector<int> result;
        // ... load result with data
        return std::move(result);       // bad; just write "return result;"
    }

Never write `return move(local_variable);`, because the language already knows the variable is a move candidate.
Writing `move` in this code won't help, and can actually be detrimental because on some compilers it interferes with RVO (the return value optimization) by creating an additional reference alias to the local variable.


##### Example, bad

    vector<int> v = std::move(make_vector());   // bad; the std::move is entirely redundant

Never write `move` on a returned value such as `x = move(f());` where `f` returns by value.
The language already knows that a returned value is a temporary object that can be moved from.

##### Example

    void mover(X&& x) {
        call_something(std::move(x));         // ok
        call_something(std::forward<X>(x));   // bad, don't std::forward an rvalue reference
        call_something(x);                    // suspicious, why not std::move?
    }

    template<class T>
    void forwarder(T&& t) {
        call_something(std::move(t));         // bad, don't std::move a forwarding reference
        call_something(std::forward<T>(t));   // ok
        call_something(t);                    // suspicious, why not std::forward?
    }

##### Enforcement

* Flag use of `std::move(x)` where `x` is an rvalue or the language will already treat it as an rvalue, including `return std::move(local_variable);` and `std::move(f())` on a function that returns by value.
* Flag functions taking an `S&&` parameter if there is no `const S&` overload to take care of lvalues.
* Flag a `std::move`s argument passed to a parameter, except when the parameter type is one of the following: an `X&&` rvalue reference; a `T&&` forwarding reference where `T` is a template parameter type; or by value and the type is move-only.
* Flag when `std::move` is applied to a forwarding reference (`T&&` where `T` is a template parameter type). Use `std::forward` instead.
* Flag when `std::move` is applied to other than an rvalue reference. (More general case of the previous rule to cover the non-forwarding cases.)
* Flag when `std::forward` is applied to an rvalue reference (`X&&` where `X` is a concrete type). Use `std::move` instead.
* Flag when `std::forward` is applied to other than a forwarding reference. (More general case of the previous rule to cover the non-moving cases.)
* Flag when an object is potentially moved from and the next operation is a `const` operation; there should first be an intervening non-`const` operation, ideally assignment, to first reset the object's value.

### <a name="Res-new"></a>ES.60: Avoid `new` and `delete` outside resource management functions

##### Reason

Direct resource management in application code is error-prone and tedious.

##### Note

This is also known as the rule of "No naked `new`!"

##### Example, bad

    void f(int n)
    {
        auto p = new X[n];   // n default constructed Xs
        // ...
        delete[] p;
    }

There can be code in the `...` part that causes the `delete` never to happen.

**See also**: [R: Resource management](#S-resource)

##### Enforcement

Flag naked `new`s and naked `delete`s.

### <a name="Res-del"></a>ES.61: Delete arrays using `delete[]` and non-arrays using `delete`

##### Reason

That's what the language requires and mistakes can lead to resource release errors and/or memory corruption.

##### Example, bad

    void f(int n)
    {
        auto p = new X[n];   // n default constructed Xs
        // ...
        delete p;   // error: just delete the object p, rather than delete the array p[]
    }

##### Note

This example not only violates the [no naked `new` rule](#Res-new) as in the previous example, it has many more problems.

##### Enforcement

* If the `new` and the `delete` are in the same scope, mistakes can be flagged.
* If the `new` and the `delete` are in a constructor/destructor pair, mistakes can be flagged.

### <a name="Res-arr2"></a>ES.62: Don't compare pointers into different arrays

##### Reason

The result of doing so is undefined.

##### Example, bad

    void f()
    {
        int a1[7];
        int a2[9];
        if (&a1[5] < &a2[7]) {}       // bad: undefined
        if (0 < &a1[5] - &a2[7]) {}   // bad: undefined
    }

##### Note

This example has many more problems.

##### Enforcement

???

### <a name="Res-slice"></a>ES.63: Don't slice

##### Reason

Slicing -- that is, copying only part of an object using assignment or initialization -- most often leads to errors because
the object was meant to be considered as a whole.
In the rare cases where the slicing was deliberate the code can be surprising.

##### Example

    class Shape { /* ... */ };
    class Circle : public Shape { /* ... */ Point c; int r; };

    Circle c {{0, 0}, 42};
    Shape s {c};    // copy construct only the Shape part of Circle
    s = c;          // or copy assign only the Shape part of Circle

    void assign(const Shape& src, Shape& dest) {
        dest = src;
    }
    Circle c2 {{1, 1}, 43};
    assign(c, c2);   // oops, not the whole state is transferred
    assert(c == c2); // if we supply copying, we should also provide comparison,
                     // but this will likely return false

The result will be meaningless because the center and radius will not be copied from `c` into `s`.
The first defense against this is to [define the base class `Shape` not to allow this](#Rc-copy-virtual).

##### Alternative

If you mean to slice, define an explicit operation to do so.
This saves readers from confusion.
For example:

    class Smiley : public Circle {
        public:
        Circle copy_circle();
        // ...
    };

    Smiley sm { /* ... */ };
    Circle c1 {sm};  // ideally prevented by the definition of Circle
    Circle c2 {sm.copy_circle()};

##### Enforcement

Warn against slicing.

### <a name="Res-construct"></a>ES.64: Use the `T{e}`notation for construction

##### Reason

The `T{e}` construction syntax makes it explicit that construction is desired.
The `T{e}` construction syntax doesn't allow narrowing.
`T{e}` is the only safe and general expression for constructing a value of type `T` from an expression `e`.
The casts notations `T(e)` and `(T)e` are neither safe nor general.

##### Example

For built-in types, the construction notation protects against narrowing and reinterpretation

    void use(char ch, int i, double d, char* p, long long lng)
    {
        int x1 = int{ch};     // OK, but redundant
        int x2 = int{d};      // error: double->int narrowing; use a cast if you need to
        int x3 = int{p};      // error: pointer to->int; use a reinterpret_cast if you really need to
        int x4 = int{lng};    // error: long long->int narrowing; use a cast if you need to

        int y1 = int(ch);     // OK, but redundant
        int y2 = int(d);      // bad: double->int narrowing; use a cast if you need to
        int y3 = int(p);      // bad: pointer to->int; use a reinterpret_cast if you really need to
        int y4 = int(lng);    // bad: long long->int narrowing; use a cast if you need to

        int z1 = (int)ch;     // OK, but redundant
        int z2 = (int)d;      // bad: double->int narrowing; use a cast if you need to
        int z3 = (int)p;      // bad: pointer to->int; use a reinterpret_cast if you really need to
        int z4 = (int)lng;    // bad: long long->int narrowing; use a cast if you need to
    }

The integer to/from pointer conversions are implementation defined when using the `T(e)` or `(T)e` notations, and non-portable
between platforms with different integer and pointer sizes.

##### Note

[Avoid casts](#Res-casts) (explicit type conversion) and if you must [prefer named casts](#Res-casts-named).

##### Note

When unambiguous, the `T` can be left out of `T{e}`.

    complex<double> f(complex<double>);

    auto z = f({2*pi, 1});

##### Note

The construction notation is the most general [initializer notation](#Res-list).

##### Exception

`std::vector` and other containers were defined before we had `{}` as a notation for construction.
Consider:

    vector<string> vs {10};                           // ten empty strings
    vector<int> vi1 {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};  // ten elements 1..10
    vector<int> vi2 {10};                             // one element with the value 10

How do we get a `vector` of 10 default initialized `int`s?

    vector<int> v3(10); // ten elements with value 0

The use of `()` rather than `{}` for number of elements is conventional (going back to the early 1980s), hard to change, but still
a design error: for a container where the element type can be confused with the number of elements, we have an ambiguity that
must be resolved.
The conventional resolution is to interpret `{10}` as a list of one element and use `(10)` to distinguish a size.

This mistake need not be repeated in new code.
We can define a type to represent the number of elements:

    struct Count { int n; };

    template<typename T>
    class Vector {
    public:
        Vector(Count n);                     // n default-initialized elements
        Vector(initializer_list<T> init);    // init.size() elements
        // ...
    };

    Vector<int> v1{10};
    Vector<int> v2{Count{10}};
    Vector<Count> v3{Count{10}};    // yes, there is still a very minor problem

The main problem left is to find a suitable name for `Count`.

##### Enforcement

Flag the C-style `(T)e` and functional-style `T(e)` casts.


### <a name="Res-deref"></a>ES.65: Don't dereference an invalid pointer

##### Reason

Dereferencing an invalid pointer, such as `nullptr`, is undefined behavior, typically leading to immediate crashes,
wrong results, or memory corruption.

##### Note

This rule is an obvious and well-known language rule, but can be hard to follow.
It takes good coding style, library support, and static analysis to eliminate violations without major overhead.
This is a major part of the discussion of [C++'s resource- and type-safety model](#Stroustrup15).

**See also**:

* Use [RAII](#Rr-raii) to avoid lifetime problems.
* Use [unique_ptr](#Rf-unique_ptr) to avoid lifetime problems.
* Use [shared_ptr](#Rf-shared_ptr) to avoid lifetime problems.
* Use [references](#Rf-ptr-ref) when `nullptr` isn't a possibility.
* Use [not_null](#Rf-not_null) to catch unexpected `nullptr` early.
* Use the [bounds profile](#SS-bounds) to avoid range errors.


##### Example

    void f()
    {
        int x = 0;
        int* p = &x;

        if (condition()) {
            int y = 0;
            p = &y;
        } // invalidates p

        *p = 42;            // BAD, p might be invalid if the branch was taken
    }

To resolve the problem, either extend the lifetime of the object the pointer is intended to refer to, or shorten the lifetime of the pointer (move the dereference to before the pointed-to object's lifetime ends).

    void f1()
    {
        int x = 0;
        int* p = &x;

        int y = 0;
        if (condition()) {
            p = &y;
        }

        *p = 42;            // OK, p points to x or y and both are still in scope
    }

Unfortunately, most invalid pointer problems are harder to spot and harder to fix.

##### Example

    void f(int* p)
    {
        int x = *p; // BAD: how do we know that p is valid?
    }

There is a huge amount of such code.
Most works -- after lots of testing -- but in isolation it is impossible to tell whether `p` could be the `nullptr`.
Consequently, this is also a major source of errors.
There are many approaches to dealing with this potential problem:

    void f1(int* p) // deal with nullptr
    {
        if (!p) {
            // deal with nullptr (allocate, return, throw, make p point to something, whatever
        }
        int x = *p;
    }

There are two potential problems with testing for `nullptr`:

* it is not always obvious what to do what to do if we find `nullptr`
* the test can be redundant and/or relatively expensive
* it is not obvious if the test is to protect against a violation or part of the required logic.


    void f2(int* p) // state that p is not supposed to be nullptr
    {
        assert(p);
        int x = *p;
    }

This would carry a cost only when the assertion checking was enabled and would give a compiler/analyzer useful information.
This would work even better if/when C++ gets direct support for contracts:

    void f3(int* p) // state that p is not supposed to be nullptr
        [[expects: p]]
    {
        int x = *p;
    }

Alternatively, we could use `gsl::not_null` to ensure that `p` is not the `nullptr`.

    void f(not_null<int*> p)
    {
        int x = *p;
    }

These remedies take care of `nullptr` only.
Remember that there are other ways of getting an invalid pointer.

##### Example

    void f(int* p)  // old code, doesn't use owner
    {
        delete p;
    }

    void g()        // old code: uses naked new
    {
        auto q = new int{7};
        f(q);
        int x = *q; // BAD: dereferences invalid pointer
    }

##### Example

    void f()
    {
        vector<int> v(10);
        int* p = &v[5];
        v.push_back(99); // could reallocate v's elements
        int x = *p; // BAD: dereferences potentially invalid pointer
    }

##### Enforcement

This rule is part of the [lifetime safety profile](#SS-lifetime)

* Flag a dereference of a pointer that points to an object that has gone out of scope
* Flag a dereference of a pointer that may have been invalidated by assigning a `nullptr`
* Flag a dereference of a pointer that may have been invalidated by a `delete`
* Flag a dereference to a pointer to a container element that may have been invalidated by dereference


## ES.stmt: Statements

Statements control the flow of control (except for function calls and exception throws, which are expressions).

### <a name="Res-switch-if"></a>ES.70: Prefer a `switch`-statement to an `if`-statement when there is a choice

##### Reason

* Readability.
* Efficiency: A `switch` compares against constants and is usually better optimized than a series of tests in an `if`-`then`-`else` chain.
* A `switch` enables some heuristic consistency checking. For example, have all values of an `enum` been covered? If not, is there a `default`?

##### Example

    void use(int n)
    {
        switch (n) {   // good
        case 0:
            // ...
            break;
        case 7:
            // ...
            break;
        default:
            // ...
            break;
        }
    }

rather than:

    void use2(int n)
    {
        if (n == 0)   // bad: if-then-else chain comparing against a set of constants
            // ...
        else if (n == 7)
            // ...
    }

##### Enforcement

Flag `if`-`then`-`else` chains that check against constants (only).

### <a name="Res-for-range"></a>ES.71: Prefer a range-`for`-statement to a `for`-statement when there is a choice

##### Reason

Readability. Error prevention. Efficiency.

##### Example

    for (gsl::index i = 0; i < v.size(); ++i)   // bad
            cout << v[i] << '\n';

    for (auto p = v.begin(); p != v.end(); ++p)   // bad
        cout << *p << '\n';

    for (auto& x : v)    // OK
        cout << x << '\n';

    for (gsl::index i = 1; i < v.size(); ++i) // touches two elements: can't be a range-for
        cout << v[i] + v[i - 1] << '\n';

    for (gsl::index i = 0; i < v.size(); ++i) // possible side effect: can't be a range-for
        cout << f(v, &v[i]) << '\n';

    for (gsl::index i = 0; i < v.size(); ++i) { // body messes with loop variable: can't be a range-for
        if (i % 2 == 0)
            continue;   // skip even elements
        else
            cout << v[i] << '\n';
    }

A human or a good static analyzer may determine that there really isn't a side effect on `v` in `f(v, &v[i])` so that the loop can be rewritten.

"Messing with the loop variable" in the body of a loop is typically best avoided.

##### Note

Don't use expensive copies of the loop variable of a range-`for` loop:

    for (string s : vs) // ...

This will copy each elements of `vs` into `s`. Better:

    for (string& s : vs) // ...

Better still, if the loop variable isn't modified or copied:

    for (const string& s : vs) // ...

##### Enforcement

Look at loops, if a traditional loop just looks at each element of a sequence, and there are no side effects on what it does with the elements, rewrite the loop to a ranged-`for` loop.

### <a name="Res-for-while"></a>ES.72: Prefer a `for`-statement to a `while`-statement when there is an obvious loop variable

##### Reason

Readability: the complete logic of the loop is visible "up front". The scope of the loop variable can be limited.

##### Example

    for (gsl::index i = 0; i < vec.size(); i++) {
        // do work
    }

##### Example, bad

    int i = 0;
    while (i < vec.size()) {
        // do work
        i++;
    }

##### Enforcement

???

### <a name="Res-while-for"></a>ES.73: Prefer a `while`-statement to a `for`-statement when there is no obvious loop variable

##### Reason

Readability.

##### Example

    int events = 0;
    for (; wait_for_event(); ++events) {  // bad, confusing
        // ...
    }

The "event loop" is misleading because the `events` counter has nothing to do with the loop condition (`wait_for_event()`).
Better

    int events = 0;
    while (wait_for_event()) {      // better
        ++events;
        // ...
    }

##### Enforcement

Flag actions in `for`-initializers and `for`-increments that do not relate to the `for`-condition.

### <a name="Res-for-init"></a>ES.74: Prefer to declare a loop variable in the initializer part of a `for`-statement

##### Reason

Limit the loop variable visibility to the scope of the loop.
Avoid using the loop variable for other purposes after the loop.

##### Example

    for (int i = 0; i < 100; ++i) {   // GOOD: i var is visible only inside the loop
        // ...
    }

##### Example, don't

    int j;                            // BAD: j is visible outside the loop
    for (j = 0; j < 100; ++j) {
        // ...
    }
    // j is still visible here and isn't needed

**See also**: [Don't use a variable for two unrelated purposes](#Res-recycle)

##### Example

    for (string s; cin >> s; ) {
        cout << s << '\n';
    }

##### Enforcement

Warn when a variable modified inside the `for`-statement is declared outside the loop and not being used outside the loop.

**Discussion**: Scoping the loop variable to the loop body also helps code optimizers greatly. Recognizing that the induction variable
is only accessible in the loop body unblocks optimizations such as hoisting, strength reduction, loop-invariant code motion, etc.

### <a name="Res-do"></a>ES.75: Avoid `do`-statements

##### Reason

Readability, avoidance of errors.
The termination condition is at the end (where it can be overlooked) and the condition is not checked the first time through.

##### Example

    int x;
    do {
        cin >> x;
        // ...
    } while (x < 0);

##### Note

Yes, there are genuine examples where a `do`-statement is a clear statement of a solution, but also many bugs.

##### Enforcement

Flag `do`-statements.

### <a name="Res-goto"></a>ES.76: Avoid `goto`

##### Reason

Readability, avoidance of errors. There are better control structures for humans; `goto` is for machine generated code.

##### Exception

Breaking out of a nested loop.
In that case, always jump forwards.

    for (int i = 0; i < imax; ++i)
        for (int j = 0; j < jmax; ++j) {
            if (a[i][j] > elem_max) goto finished;
            // ...
        }
    finished:
    // ...

##### Example, bad

There is a fair amount of use of the C goto-exit idiom:

    void f()
    {
        // ...
            goto exit;
        // ...
            goto exit;
        // ...
    exit:
        // ... common cleanup code ...
    }

This is an ad-hoc simulation of destructors.
Declare your resources with handles with destructors that clean up.
If for some reason you cannot handle all cleanup with destructors for the variables used,
consider `gsl::finally()` as a cleaner and more reliable alternative to `goto exit`

##### Enforcement

* Flag `goto`. Better still flag all `goto`s that do not jump from a nested loop to the statement immediately after a nest of loops.

### <a name="Res-continue"></a>ES.77: Minimize the use of `break` and `continue` in loops

##### Reason

 In a non-trivial loop body, it is easy to overlook a `break` or a `continue`.

 A `break` in a loop has a dramatically different meaning than a `break` in a `switch`-statement
 (and you can have `switch`-statement in a loop and a loop in a `switch`-case).

##### Example

    ???

##### Alternative

Often, a loop that requires a `break` is a good candidate for a function (algorithm), in which case the `break` becomes a `return`.

    ???

Often, a loop that uses `continue` can equivalently and as clearly be expressed by an `if`-statement.

    ???

##### Note

If you really need to break out a loop, a `break` is typically better than alternatives such as [modifying the loop variable](#Res-loop-counter) or a [`goto`](#Res-goto):


##### Enforcement

???

### <a name="Res-break"></a>ES.78: Always end a non-empty `case` with a `break`

##### Reason

 Accidentally leaving out a `break` is a fairly common bug.
 A deliberate fallthrough is a maintenance hazard.

##### Example

    switch (eventType) {
    case Information:
        update_status_bar();
        break;
    case Warning:
        write_event_log();
        // Bad - implicit fallthrough
    case Error:
        display_error_window();
        break;
    }

It is easy to overlook the fallthrough. Be explicit:

    switch (eventType) {
    case Information:
        update_status_bar();
        break;
    case Warning:
        write_event_log();
        // fallthrough
    case Error:
        display_error_window();
        break;
    }

In C++17, use a `[[fallthrough]]` annotation:

    switch (eventType) {
    case Information:
        update_status_bar();
        break;
    case Warning:
        write_event_log();
        [[fallthrough]];        // C++17
    case Error:
        display_error_window();
        break;
    }

##### Note

Multiple case labels of a single statement is OK:

    switch (x) {
    case 'a':
    case 'b':
    case 'f':
        do_something(x);
        break;
    }

##### Enforcement

Flag all fallthroughs from non-empty `case`s.

### <a name="Res-default"></a>ES.79: Use `default` to handle common cases (only)

##### Reason

 Code clarity.
 Improved opportunities for error detection.

##### Example

    enum E { a, b, c , d };

    void f1(E x)
    {
        switch (x) {
        case a:
            do_something();
            break;
        case b:
            do_something_else();
            break;
        default:
            take_the_default_action();
            break;
        }
    }

Here it is clear that there is a default action and that cases `a` and `b` are special.

##### Example

But what if there is no default action and you mean to handle only specific cases?
In that case, have an empty default or else it is impossible to know if you meant to handle all cases:

    void f2(E x)
    {
        switch (x) {
        case a:
            do_something();
            break;
        case b:
            do_something_else();
            break;
        default:
            // do nothing for the rest of the cases
            break;
        }
    }

If you leave out the `default`, a maintainer and/or a compiler may reasonably assume that you intended to handle all cases:

    void f2(E x)
    {
        switch (x) {
        case a:
            do_something();
            break;
        case b:
        case c:
            do_something_else();
            break;
        }
    }

Did you forget case `d` or deliberately leave it out?
Forgetting a case typically happens when a case is added to an enumeration and the person doing so fails to add it to every
switch over the enumerators.

##### Enforcement

Flag `switch`-statements over an enumeration that don't handle all enumerators and do not have a `default`.
This may yield too many false positives in some code bases; if so, flag only `switch`es that handle most but not all cases
(that was the strategy of the very first C++ compiler).

### <a name="Res-noname"></a>ES.84: Don't try to declare a local variable with no name

##### Reason

There is no such thing.
What looks to a human like a variable without a name is to the compiler a statement consisting of a temporary that immediately goes out of scope.

##### Example, bad

    void f()
    {
        lock<mutex>{mx};   // Bad
        // ...
    }

This declares an unnamed `lock` object that immediately goes out of scope at the point of the semicolon.
This is not an uncommon mistake.
In particular, this particular example can lead to hard-to find race conditions.

##### Note

Unnamed function arguments are fine.

##### Enforcement

Flag statements that are just a temporary.

### <a name="Res-empty"></a>ES.85: Make empty statements visible

##### Reason

Readability.

##### Example

    for (i = 0; i < max; ++i);   // BAD: the empty statement is easily overlooked
    v[i] = f(v[i]);

    for (auto x : v) {           // better
        // nothing
    }
    v[i] = f(v[i]);

##### Enforcement

Flag empty statements that are not blocks and don't contain comments.

### <a name="Res-loop-counter"></a>ES.86: Avoid modifying loop control variables inside the body of raw for-loops

##### Reason

The loop control up front should enable correct reasoning about what is happening inside the loop. Modifying loop counters in both the iteration-expression and inside the body of the loop is a perennial source of surprises and bugs.

##### Example

    for (int i = 0; i < 10; ++i) {
        // no updates to i -- ok
    }

    for (int i = 0; i < 10; ++i) {
        //
        if (/* something */) ++i; // BAD
        //
    }

    bool skip = false;
    for (int i = 0; i < 10; ++i) {
        if (skip) { skip = false; continue; }
        //
        if (/* something */) skip = true;  // Better: using two variables for two concepts.
        //
    }

##### Enforcement

Flag variables that are potentially updated (have a non-`const` use) in both the loop control iteration-expression and the loop body.


### <a name="Res-if"></a>ES.87: Don't add redundant `==` or `!=` to conditions

##### Reason

Doing so avoids verbosity and eliminates some opportunities for mistakes.
Helps make style consistent and conventional.

##### Example

By definition, a condition in an `if`-statement, `while`-statement, or a `for`-statement selects between `true` and `false`.
A numeric value is compared to `0` and a pointer value to `nullptr`.

    // These all mean "if `p` is not `nullptr`"
    if (p) { ... }            // good
    if (p != 0) { ... }       // redundant `!=0`; bad: don't use 0 for pointers
    if (p != nullptr) { ... } // redundant `!=nullptr`, not recommended

Often, `if (p)` is read as "if `p` is valid" which is a direct expression of the programmers intent,
whereas `if (p != nullptr)` would be a long-winded workaround.

##### Example

This rule is especially useful when a declaration is used as a condition

    if (auto pc = dynamic_cast<Circle>(ps)) { ... } // execute if ps points to a kind of Circle, good

    if (auto pc = dynamic_cast<Circle>(ps); pc != nullptr) { ... } // not recommended

##### Example

Note that implicit conversions to bool are applied in conditions.
For example:

    for (string s; cin >> s; ) v.push_back(s);

This invokes `istream`'s `operator bool()`.

##### Note

Explicit comparison of an integer to `0` is in general not redundant.
The reason is that (as opposed to pointers and Booleans) an integer often has more than two reasonable values.
Furthermore `0` (zero) is often used to indicate success.
Consequently, it is best to be specific about the comparison.

    void f(int i)
    {
        if (i)            // suspect
        // ...
        if (i == success) // possibly better
        // ...
    }

Always remember that an integer can have more than two values.

##### Example, bad

It has been noted that

    if(strcmp(p1, p2)) { ... }   // are the two C-style strings equal? (mistake!)

is a common beginners error.
If you use C-style strings, you must know the `<cstring>` functions well.
Being verbose and writing

    if(strcmp(p1, p2) != 0) { ... }   // are the two C-style strings equal? (mistake!)

would not in itself save you.

##### Note

The opposite condition is most easily expressed using a negation:

    // These all mean "if `p` is `nullptr`"
    if (!p) { ... }           // good
    if (p == 0) { ... }       // redundant `== 0`; bad: don't use `0` for pointers
    if (p == nullptr) { ... } // redundant `== nullptr`, not recommended

##### Enforcement

Easy, just check for redundant use of `!=` and `==` in conditions.



## <a name="SS-numbers"></a>Arithmetic

### <a name="Res-mix"></a>ES.100: Don't mix signed and unsigned arithmetic

##### Reason

Avoid wrong results.

##### Example

    int x = -3;
    unsigned int y = 7;

    cout << x - y << '\n';  // unsigned result, possibly 4294967286
    cout << x + y << '\n';  // unsigned result: 4
    cout << x * y << '\n';  // unsigned result, possibly 4294967275

It is harder to spot the problem in more realistic examples.

##### Note

Unfortunately, C++ uses signed integers for array subscripts and the standard library uses unsigned integers for container subscripts.
This precludes consistency. Use `gsl::index` for subscripts; [see ES.107](#Res-subscripts).

##### Enforcement

* Compilers already know and sometimes warn.
* (To avoid noise) Do not flag on a mixed signed/unsigned comparison where one of the arguments is `sizeof` or a call to container `.size()` and the other is `ptrdiff_t`.


### <a name="Res-unsigned"></a>ES.101: Use unsigned types for bit manipulation

##### Reason

Unsigned types support bit manipulation without surprises from sign bits.

##### Example

    unsigned char x = 0b1010'1010;
    unsigned char y = ~x;   // y == 0b0101'0101;

##### Note

Unsigned types can also be useful for modulo arithmetic.
However, if you want modulo arithmetic add
comments as necessary noting the reliance on wraparound behavior, as such code
can be surprising for many programmers.

##### Enforcement

* Just about impossible in general because of the use of unsigned subscripts in the standard library
* ???

### <a name="Res-signed"></a>ES.102: Use signed types for arithmetic

##### Reason

Because most arithmetic is assumed to be signed;
`x - y` yields a negative number when `y > x` except in the rare cases where you really want modulo arithmetic.

##### Example

Unsigned arithmetic can yield surprising results if you are not expecting it.
This is even more true for mixed signed and unsigned arithmetic.

    template<typename T, typename T2>
    T subtract(T x, T2 y)
    {
        return x - y;
    }

    void test()
    {
        int s = 5;
        unsigned int us = 5;
        cout << subtract(s, 7) << '\n';       // -2
        cout << subtract(us, 7u) << '\n';     // 4294967294
        cout << subtract(s, 7u) << '\n';      // -2
        cout << subtract(us, 7) << '\n';      // 4294967294
        cout << subtract(s, us + 2) << '\n';  // -2
        cout << subtract(us, s + 2) << '\n';  // 4294967294
    }

Here we have been very explicit about what's happening,
but if you had seen `us - (s + 2)` or `s += 2; ...; us - s`, would you reliably have suspected that the result would print as `4294967294`?

##### Exception

Use unsigned types if you really want modulo arithmetic - add
comments as necessary noting the reliance on overflow behavior, as such code
is going to be surprising for many programmers.

##### Example

The standard library uses unsigned types for subscripts.
The built-in array uses signed types for subscripts.
This makes surprises (and bugs) inevitable.

    int a[10];
    for (int i = 0; i < 10; ++i) a[i] = i;
    vector<int> v(10);
    // compares signed to unsigned; some compilers warn, but we should not
    for (gsl::index i = 0; i < v.size(); ++i) v[i] = i;

    int a2[-2];         // error: negative size

    // OK, but the number of ints (4294967294) is so large that we should get an exception
    vector<int> v2(-2);

 Use `gsl::index` for subscripts; [see ES.107](#Res-subscripts).

##### Enforcement

* Flag mixed signed and unsigned arithmetic
* Flag results of unsigned arithmetic assigned to or printed as signed.
* Flag negative literals (e.g. `-2`) used as container subscripts.
* (To avoid noise) Do not flag on a mixed signed/unsigned comparison where one of the arguments is `sizeof` or a call to container `.size()` and the other is `ptrdiff_t`.


### <a name="Res-overflow"></a>ES.103: Don't overflow

##### Reason

Overflow usually makes your numeric algorithm meaningless.
Incrementing a value beyond a maximum value can lead to memory corruption and undefined behavior.

##### Example, bad

    int a[10];
    a[10] = 7;   // bad

    int n = 0;
    while (n++ < 10)
        a[n - 1] = 9; // bad (twice)

##### Example, bad

    int n = numeric_limits<int>::max();
    int m = n + 1;   // bad

##### Example, bad

    int area(int h, int w) { return h * w; }

    auto a = area(10'000'000, 100'000'000);   // bad

##### Exception

Use unsigned types if you really want modulo arithmetic.

**Alternative**: For critical applications that can afford some overhead, use a range-checked integer and/or floating-point type.

##### Enforcement

???

### <a name="Res-underflow"></a>ES.104: Don't underflow

##### Reason

Decrementing a value beyond a minimum value can lead to memory corruption and undefined behavior.

##### Example, bad

    int a[10];
    a[-2] = 7;   // bad

    int n = 101;
    while (n--)
        a[n - 1] = 9;   // bad (twice)

##### Exception

Use unsigned types if you really want modulo arithmetic.

##### Enforcement

???

### <a name="Res-zero"></a>ES.105: Don't divide by zero

##### Reason

The result is undefined and probably a crash.

##### Note

This also applies to `%`.

##### Example, bad

    double divide(int a, int b) {
        // BAD, should be checked (e.g., in a precondition)
        return a / b;
    }

##### Example, good

    double divide(int a, int b) {
        // good, address via precondition (and replace with contracts once C++ gets them)
        Expects(b != 0);
        return a / b;
    }

    double divide(int a, int b) {
        // good, address via check
        return b ? a / b : quiet_NaN<double>();
    }

**Alternative**: For critical applications that can afford some overhead, use a range-checked integer and/or floating-point type.

##### Enforcement

* Flag division by an integral value that could be zero


### <a name="Res-nonnegative"></a>ES.106: Don't try to avoid negative values by using `unsigned`

##### Reason

Choosing `unsigned` implies many changes to the usual behavior of integers, including modulo arithmetic,
can suppress warnings related to overflow,
and opens the door for errors related to signed/unsigned mixes.
Using `unsigned` doesn't actually eliminate the possibility of negative values.

##### Example

    unsigned int u1 = -2;   // Valid: the value of u1 is 4294967294
    int i1 = -2;
    unsigned int u2 = i1;   // Valid: the value of u2 is 4294967294
    int i2 = u2;            // Valid: the value of i2 is -2

These problems with such (perfectly legal) constructs are hard to spot in real code and are the source of many real-world errors.
Consider:

    unsigned area(unsigned height, unsigned width) { return height*width; } // [see also](#Ri-expects)
    // ...
    int height;
    cin >> height;
    auto a = area(height, 2);   // if the input is -2 a becomes 4294967292

Remember that `-1` when assigned to an `unsigned int` becomes the largest `unsigned int`.
Also, since unsigned arithmetic is modulo arithmetic the multiplication didn't overflow, it wrapped around.

##### Example

    unsigned max = 100000;    // "accidental typo", I mean to say 10'000
    unsigned short x = 100;
    while (x < max) x += 100; // infinite loop

Had `x` been a signed `short`, we could have warned about the undefined behavior upon overflow.

##### Alternatives

* use signed integers and check for `x >= 0`
* use a positive integer type
* use an integer subrange type
* `Assert(-1 < x)`

For example

    struct Positive {
        int val;
        Positive(int x) :val{x} { Assert(0 < x); }
        operator int() { return val; }
    };

    int f(Positive arg) { return arg; }

    int r1 = f(2);
    int r2 = f(-2);  // throws

##### Note

???

##### Enforcement

Hard: there is a lot of code using `unsigned` and we don't offer a practical positive number type.


### <a name="Res-subscripts"></a>ES.107: Don't use `unsigned` for subscripts, prefer `gsl::index`

##### Reason

To avoid signed/unsigned confusion.
To enable better optimization.
To enable better error detection.
To avoid the pitfalls with `auto` and `int`.

##### Example, bad

    vector<int> vec = /*...*/;

    for (int i = 0; i < vec.size(); i += 2)                    // may not be big enough
        cout << vec[i] << '\n';
    for (unsigned i = 0; i < vec.size(); i += 2)               // risk wraparound
        cout << vec[i] << '\n';
    for (auto i = 0; i < vec.size(); i += 2)                   // may not be big enough
        cout << vec[i] << '\n';
    for (vector<int>::size_type i = 0; i < vec.size(); i += 2) // verbose
        cout << vec[i] << '\n';
    for (auto i = vec.size()-1; i >= 0; i -= 2)                // bug
        cout << vec[i] << '\n';
    for (int i = vec.size()-1; i >= 0; i -= 2)                 // may not be big enough
        cout << vec[i] << '\n';

##### Example, good

    vector<int> vec = /*...*/;

    for (gsl::index i = 0; i < vec.size(); i += 2)             // ok
        cout << vec[i] << '\n';
    for (gsl::index i = vec.size()-1; i >= 0; i -= 2)          // ok
        cout << vec[i] << '\n';

##### Note

The built-in array uses signed subscripts.
The standard-library containers use unsigned subscripts.
Thus, no perfect and fully compatible solution is possible (unless and until the standard-library containers change to use signed subscripts someday in the future).
Given the known problems with unsigned and signed/unsigned mixtures, better stick to (signed) integers of a sufficient size, which is guaranteed by `gsl::index`.

##### Example

    template<typename T>
    struct My_container {
    public:
        // ...
        T& operator[](gsl::index i);    // not unsigned
        // ...
    };

##### Example

    ??? demonstrate improved code generation and potential for error detection ???

##### Alternatives

Alternatives for users

* use algorithms
* use range-for
* use iterators/pointers

##### Enforcement

* Very tricky as long as the standard-library containers get it wrong.
* (To avoid noise) Do not flag on a mixed signed/unsigned comparison where one of the arguments is `sizeof` or a call to container `.size()` and the other is `ptrdiff_t`.




# <a name="S-performance"></a>Per: Performance

??? should this section be in the main guide???

This section contains rules for people who need high performance or low-latency.
That is, these are rules that relate to how to use as little time and as few resources as possible to achieve a task in a predictably short time.
The rules in this section are more restrictive and intrusive than what is needed for many (most) applications.
Do not blindly try to follow them in general code: achieving the goals of low latency requires extra work.

Performance rule summary:

* [Per.1: Don't optimize without reason](#Rper-reason)
* [Per.2: Don't optimize prematurely](#Rper-Knuth)
* [Per.3: Don't optimize something that's not performance critical](#Rper-critical)
* [Per.4: Don't assume that complicated code is necessarily faster than simple code](#Rper-simple)
* [Per.5: Don't assume that low-level code is necessarily faster than high-level code](#Rper-low)
* [Per.6: Don't make claims about performance without measurements](#Rper-measure)
* [Per.7: Design to enable optimization](#Rper-efficiency)
* [Per.10: Rely on the static type system](#Rper-type)
* [Per.11: Move computation from run time to compile time](#Rper-Comp)
* [Per.12: Eliminate redundant aliases](#Rper-alias)
* [Per.13: Eliminate redundant indirections](#Rper-indirect)
* [Per.14: Minimize the number of allocations and deallocations](#Rper-alloc)
* [Per.15: Do not allocate on a critical branch](#Rper-alloc0)
* [Per.16: Use compact data structures](#Rper-compact)
* [Per.17: Declare the most used member of a time-critical struct first](#Rper-struct)
* [Per.18: Space is time](#Rper-space)
* [Per.19: Access memory predictably](#Rper-access)
* [Per.30: Avoid context switches on the critical path](#Rper-context)

### <a name="Rper-reason"></a>Per.1: Don't optimize without reason

##### Reason

If there is no need for optimization, the main result of the effort will be more errors and higher maintenance costs.

##### Note

Some people optimize out of habit or because it's fun.

???

### <a name="Rper-Knuth"></a>Per.2: Don't optimize prematurely

##### Reason

Elaborately optimized code is usually larger and harder to change than unoptimized code.

???

### <a name="Rper-critical"></a>Per.3: Don't optimize something that's not performance critical

##### Reason

Optimizing a non-performance-critical part of a program has no effect on system performance.

##### Note

If your program spends most of its time waiting for the web or for a human, optimization of in-memory computation is probably useless.

Put another way: If your program spends 4% of its processing time doing
computation A and 40% of its time doing computation B, a 50% improvement on A is
only as impactful as a 5% improvement on B. (If you don't even know how much
time is spent on A or B, see <a href="#Rper-reason">Per.1</a> and <a
href="#Rper-Knuth">Per.2</a>.)

### <a name="Rper-simple"></a>Per.4: Don't assume that complicated code is necessarily faster than simple code

##### Reason

Simple code can be very fast. Optimizers sometimes do marvels with simple code

##### Example, good

    // clear expression of intent, fast execution

    vector<uint8_t> v(100000);

    for (auto& c : v)
        c = ~c;

##### Example, bad

    // intended to be faster, but is actually slower

    vector<uint8_t> v(100000);

    for (size_t i = 0; i < v.size(); i += sizeof(uint64_t))
    {
        uint64_t& quad_word = *reinterpret_cast<uint64_t*>(&v[i]);
        quad_word = ~quad_word;
    }

##### Note

???

???

### <a name="Rper-low"></a>Per.5: Don't assume that low-level code is necessarily faster than high-level code

##### Reason

Low-level code sometimes inhibits optimizations. Optimizers sometimes do marvels with high-level code.

##### Note

???

???

### <a name="Rper-measure"></a>Per.6: Don't make claims about performance without measurements

##### Reason

The field of performance is littered with myth and bogus folklore.
Modern hardware and optimizers defy naive assumptions; even experts are regularly surprised.

##### Note

Getting good performance measurements can be hard and require specialized tools.

##### Note

A few simple microbenchmarks using Unix `time` or the standard-library `<chrono>` can help dispel the most obvious myths.
If you can't measure your complete system accurately, at least try to measure a few of your key operations and algorithms.
A profiler can help tell you which parts of your system are performance critical.
Often, you will be surprised.

???

### <a name="Rper-efficiency"></a>Per.7: Design to enable optimization

##### Reason

Because we often need to optimize the initial design.
Because a design that ignores the possibility of later improvement is hard to change.

##### Example

From the C (and C++) standard:

    void qsort (void* base, size_t num, size_t size, int (*compar)(const void*, const void*));

When did you even want to sort memory?
Really, we sort sequences of elements, typically stored in containers.
A call to `qsort` throws away much useful information (e.g., the element type), forces the user to repeat information
already known (e.g., the element size), and forces the user to write extra code (e.g., a function to compare `double`s).
This implies added work for the programmer, is error-prone, and deprives the compiler of information needed for optimization.

    double data[100];
    // ... fill a ...

    // 100 chunks of memory of sizeof(double) starting at
    // address data using the order defined by compare_doubles
    qsort(data, 100, sizeof(double), compare_doubles);

From the point of view of interface design is that `qsort` throws away useful information.

We can do better (in C++98)

    template<typename Iter>
        void sort(Iter b, Iter e);  // sort [b:e)

    sort(data, data + 100);

Here, we use the compiler's knowledge about the size of the array, the type of elements, and how to compare `double`s.

With C++11 plus [concepts](#SS-concepts), we can do better still

    // Sortable specifies that c must be a
    // random-access sequence of elements comparable with <
    void sort(Sortable& c);

    sort(c);

The key is to pass sufficient information for a good implementation to be chosen.
In this, the `sort` interfaces shown here still have a weakness:
They implicitly rely on the element type having less-than (`<`) defined.
To complete the interface, we need a second version that accepts a comparison criteria:

    // compare elements of c using p
    void sort(Sortable& c, Predicate<Value_type<Sortable>> p);

The standard-library specification of `sort` offers those two versions,
but the semantics is expressed in English rather than code using concepts.

##### Note

Premature optimization is said to be [the root of all evil](#Rper-Knuth), but that's not a reason to despise performance.
It is never premature to consider what makes a design amenable to improvement, and improved performance is a commonly desired improvement.
Aim to build a set of habits that by default results in efficient, maintainable, and optimizable code.
In particular, when you write a function that is not a one-off implementation detail, consider

* Information passing:
Prefer clean [interfaces](#S-interfaces) carrying sufficient information for later improvement of implementation.
Note that information flows into and out of an implementation through the interfaces we provide.
* Compact data: By default, [use compact data](#Rper-compact), such as `std::vector` and [access it in a systematic fashion](#Rper-access).
If you think you need a linked structure, try to craft the interface so that this structure isn't seen by users.
* Function argument passing and return:
Distinguish between mutable and non-mutable data.
Don't impose a resource management burden on your users.
Don't impose spurious run-time indirections on your users.
Use [conventional ways](#Rf-conventional) of passing information through an interface;
unconventional and/or "optimized" ways of passing data can seriously complicate later reimplementation.
* Abstraction:
Don't overgeneralize; a design that tries to cater for every possible use (and misuse) and defers every design decision for later
(using compile-time or run-time indirections) is usually a complicated, bloated, hard-to-understand mess.
Generalize from concrete examples, preserving performance as we generalize.
Do not generalize based on mere speculation about future needs.
The ideal is zero-overhead generalization.
* Libraries:
Use libraries with good interfaces.
If no library is available build one yourself and imitate the interface style from a good library.
The [standard library](#S-stdlib) is a good first place to look for inspiration.
* Isolation:
Isolate your code from messy and/or old-style code by providing an interface of your choosing to it.
This is sometimes called "providing a wrapper" for the useful/necessary but messy code.
Don't let bad designs "bleed into" your code.

##### Example

Consider:

    template <class ForwardIterator, class T>
    bool binary_search(ForwardIterator first, ForwardIterator last, const T& val);

`binary_search(begin(c), end(c), 7)` will tell you whether `7` is in `c` or not.
However, it will not tell you where that `7` is or whether there are more than one `7`.

Sometimes, just passing the minimal amount of information back (here, `true` or `false`) is sufficient, but a good interface passes
needed information back to the caller. Therefore, the standard library also offers

    template <class ForwardIterator, class T>
    ForwardIterator lower_bound(ForwardIterator first, ForwardIterator last, const T& val);

`lower_bound` returns an iterator to the first match if any, otherwise to the first element greater than `val`, or `last` if no such element is found.

However, `lower_bound` still doesn't return enough information for all uses, so the standard library also offers

    template <class ForwardIterator, class T>
    pair<ForwardIterator, ForwardIterator>
    equal_range(ForwardIterator first, ForwardIterator last, const T& val);

`equal_range` returns a `pair` of iterators specifying the first and one beyond last match.

    auto r = equal_range(begin(c), end(c), 7);
    for (auto p = r.first; p != r.second; ++p)
        cout << *p << '\n';

Obviously, these three interfaces are implemented by the same basic code.
They are simply three ways of presenting the basic binary search algorithm to users,
ranging from the simplest ("make simple things simple!")
to returning complete, but not always needed, information ("don't hide useful information").
Naturally, crafting such a set of interfaces requires experience and domain knowledge.

##### Note

Do not simply craft the interface to match the first implementation and the first use case you think of.
Once your first initial implementation is complete, review it; once you deploy it, mistakes will be hard to remedy.

##### Note

A need for efficiency does not imply a need for [low-level code](#Rper-low).
High-level code does not imply slow or bloated.

##### Note

Things have costs.
Don't be paranoid about costs (modern computers really are very fast),
but have a rough idea of the order of magnitude of cost of what you use.
For example, have a rough idea of the cost of
a memory access,
a function call,
a string comparison,
a system call,
a disk access,
and a message through a network.

##### Note

If you can only think of one implementation, you probably don't have something for which you can devise a stable interface.
Maybe, it is just an implementation detail - not every piece of code needs a stable interface - but pause and consider.
One question that can be useful is
"what interface would be needed if this operation should be implemented using multiple threads? be vectorized?"

##### Note

This rule does not contradict the [Don't optimize prematurely](#Rper-Knuth) rule.
It complements it encouraging developers enable later - appropriate and non-premature - optimization, if and where needed.

##### Enforcement

Tricky.
Maybe looking for `void*` function arguments will find examples of interfaces that hinder later optimization.

### <a name="Rper-type"></a>Per.10: Rely on the static type system

##### Reason

Type violations, weak types (e.g. `void*`s), and low-level code (e.g., manipulation of sequences as individual bytes) make the job of the optimizer much harder. Simple code often optimizes better than hand-crafted complex code.

???

### <a name="Rper-Comp"></a>Per.11: Move computation from run time to compile time

##### Reason

To decrease code size and run time.
To avoid data races by using constants.
To catch errors at compile time (and thus eliminate the need for error-handling code).

##### Example

    double square(double d) { return d*d; }
    static double s2 = square(2);    // old-style: dynamic initialization

    constexpr double ntimes(double d, int n)   // assume 0 <= n
    {
            double m = 1;
            while (n--) m *= d;
            return m;
    }
    constexpr double s3 {ntimes(2, 3)};  // modern-style: compile-time initialization

Code like the initialization of `s2` isn't uncommon, especially for initialization that's a bit more complicated than `square()`.
However, compared to the initialization of `s3` there are two problems:

* we suffer the overhead of a function call at run time
* `s2` just might be accessed by another thread before the initialization happens.

Note: you can't have a data race on a constant.

##### Example

Consider a popular technique for providing a handle for storing small objects in the handle itself and larger ones on the heap.

    constexpr int on_stack_max = 20;

    template<typename T>
    struct Scoped {     // store a T in Scoped
            // ...
        T obj;
    };

    template<typename T>
    struct On_heap {    // store a T on the free store
            // ...
            T* objp;
    };

    template<typename T>
    using Handle = typename std::conditional<(sizeof(T) <= on_stack_max),
                        Scoped<T>,      // first alternative
                        On_heap<T>      // second alternative
                   >::type;

    void f()
    {
        Handle<double> v1;                   // the double goes on the stack
        Handle<std::array<double, 200>> v2;  // the array goes on the free store
        // ...
    }

Assume that `Scoped` and `On_heap` provide compatible user interfaces.
Here we compute the optimal type to use at compile time.
There are similar techniques for selecting the optimal function to call.

##### Note

The ideal is {not} to try execute everything at compile time.
Obviously, most computations depend on inputs so they can't be moved to compile time,
but beyond that logical constraint is the fact that complex compile-time computation can seriously increase compile times
and complicate debugging.
It is even possible to slow down code by compile-time computation.
This is admittedly rare, but by factoring out a general computation into separate optimal sub-calculations it is possible to render the instruction cache less effective.

##### Enforcement

* Look for simple functions that might be constexpr (but are not).
* Look for functions called with all constant-expression arguments.
* Look for macros that could be constexpr.

### <a name="Rper-alias"></a>Per.12: Eliminate redundant aliases

???

### <a name="Rper-indirect"></a>Per.13: Eliminate redundant indirections

???

### <a name="Rper-alloc"></a>Per.14: Minimize the number of allocations and deallocations

???

### <a name="Rper-alloc0"></a>Per.15: Do not allocate on a critical branch

???

### <a name="Rper-compact"></a>Per.16: Use compact data structures

##### Reason

Performance is typically dominated by memory access times.

???

### <a name="Rper-struct"></a>Per.17: Declare the most used member of a time-critical struct first

???

### <a name="Rper-space"></a>Per.18: Space is time

##### Reason

Performance is typically dominated by memory access times.

???

### <a name="Rper-access"></a>Per.19: Access memory predictably

##### Reason

Performance is very sensitive to cache performance and cache algorithms favor simple (usually linear) access to adjacent data.

##### Example

    int matrix[rows][cols];

    // bad
    for (int c = 0; c < cols; ++c)
        for (int r = 0; r < rows; ++r)
            sum += matrix[r][c];

    // good
    for (int r = 0; r < rows; ++r)
        for (int c = 0; c < cols; ++c)
            sum += matrix[r][c];

### <a name="Rper-context"></a>Per.30: Avoid context switches on the critical path

???

# <a name="S-concurrency"></a>CP: Concurrency and parallelism

We often want our computers to do many tasks at the same time (or at least appear to do them at the same time).
The reasons for doing so vary (e.g., waiting for many events using only a single processor, processing many data streams simultaneously, or utilizing many hardware facilities)
and so do the basic facilities for expressing concurrency and parallelism.
Here, we articulate principles and rules for using the ISO standard C++ facilities for expressing basic concurrency and parallelism.

Threads are the machine-level foundation for concurrent and parallel programming.
Threads allow running multiple sections of a program independently, while sharing
the same memory. Concurrent programming is tricky,
because protecting shared data between threads is easier said than done.
Making existing single-threaded code execute concurrently can be
as trivial as adding `std::async` or `std::thread` strategically, or it can
necessitate a full rewrite, depending on whether the original code was written
in a thread-friendly way.

The concurrency/parallelism rules in this document are designed with three goals
in mind:

* To help in writing code that is amenable to being used in a threaded
  environment
* To show clean, safe ways to use the threading primitives offered by the
  standard library
* To offer guidance on what to do when concurrency and parallelism aren't giving
  the performance gains needed

It is also important to note that concurrency in C++ is an unfinished
story. C++11 introduced many core concurrency primitives, C++14 and C++17 improved on
them, and there is much interest in making the writing of
concurrent programs in C++ even easier. We expect some of the library-related
guidance here to change significantly over time.

This section needs a lot of work (obviously).
Please note that we start with rules for relative non-experts.
Real experts must wait a bit;
contributions are welcome,
but please think about the majority of programmers who are struggling to get their concurrent programs correct and performant.

Concurrency and parallelism rule summary:

* [CP.1: Assume that your code will run as part of a multi-threaded program](#Rconc-multi)
* [CP.2: Avoid data races](#Rconc-races)
* [CP.3: Minimize explicit sharing of writable data](#Rconc-data)
* [CP.4: Think in terms of tasks, rather than threads](#Rconc-task)
* [CP.8: Don't try to use `volatile` for synchronization](#Rconc-volatile)
* [CP.9: Whenever feasible use tools to validate your concurrent code](#Rconc-tools)

**See also**:

* [CP.con: Concurrency](#SScp-con)
* [CP.par: Parallelism](#SScp-par)
* [CP.mess: Message passing](#SScp-mess)
* [CP.vec: Vectorization](#SScp-vec)
* [CP.free: Lock-free programming](#SScp-free)
* [CP.etc: Etc. concurrency rules](#SScp-etc)

### <a name="Rconc-multi"></a>CP.1: Assume that your code will run as part of a multi-threaded program

##### Reason

It is hard to be certain that concurrency isn't used now or will be sometime in the future.
Code gets reused.
Libraries using threads may be used from some other part of the program.
Note that this applies most urgently to library code and least urgently to stand-alone applications.
However, thanks to the magic of cut-and-paste, code fragments can turn up in unexpected places.

##### Example

    double cached_computation(double x)
    {
        static double cached_x = 0.0;
        static double cached_result = COMPUTATION_OF_ZERO;
        double result;

        if (cached_x == x)
            return cached_result;
        result = computation(x);
        cached_x = x;
        cached_result = result;
        return result;
    }

Although `cached_computation` works perfectly in a single-threaded environment, in a multi-threaded environment the two `static` variables result in data races and thus undefined behavior.

There are several ways that this example could be made safe for a multi-threaded environment:

* Delegate concurrency concerns upwards to the caller.
* Mark the `static` variables as `thread_local` (which might make caching less effective).
* Implement concurrency control, for example, protecting the two `static` variables with a `static` lock (which might reduce performance).
* Have the caller provide the memory to be used for the cache, thereby delegating both memory allocation and concurrency concerns upwards to the caller.
* Refuse to build and/or run in a multi-threaded environment.
* Provide two implementations, one which is used in single-threaded environments and another which is used in multi-threaded environments.

##### Exception

Code that is never run in a multi-threaded environment.

Be careful: there are many examples where code that was "known" to never run in a multi-threaded program
was run as part of a multi-threaded program, often years later.
Typically, such programs lead to a painful effort to remove data races.
Therefore, code that is never intended to run in a multi-threaded environment should be clearly labeled as such and ideally come with compile or run-time enforcement mechanisms to catch those usage bugs early.

### <a name="Rconc-races"></a>CP.2: Avoid data races

##### Reason

Unless you do, nothing is guaranteed to work and subtle errors will persist.

##### Note

In a nutshell, if two threads can access the same object concurrently (without synchronization), and at least one is a writer (performing a non-`const` operation), you have a data race.
For further information of how to use synchronization well to eliminate data races, please consult a good book about concurrency.

##### Example, bad

There are many examples of data races that exist, some of which are running in
production software at this very moment. One very simple example:

    int get_id() {
      static int id = 1;
      return id++;
    }

The increment here is an example of a data race. This can go wrong in many ways,
including:

* Thread A loads the value of `id`, the OS context switches A out for some
  period, during which other threads create hundreds of IDs. Thread A is then
  allowed to run again, and `id` is written back to that location as A's read of
  `id` plus one.
* Thread A and B load `id` and increment it simultaneously.  They both get the
  same ID.

Local static variables are a common source of data races.

##### Example, bad:

    void f(fstream&  fs, regex pat)
    {
        array<double, max> buf;
        int sz = read_vec(fs, buf, max);            // read from fs into buf
        gsl::span<double> s {buf};
        // ...
        auto h1 = async([&]{ sort(par, s); });     // spawn a task to sort
        // ...
        auto h2 = async([&]{ return find_all(buf, sz, pat); });   // spawn a task to find matches
        // ...
    }

Here, we have a (nasty) data race on the elements of `buf` (`sort` will both read and write).
All data races are nasty.
Here, we managed to get a data race on data on the stack.
Not all data races are as easy to spot as this one.

##### Example, bad:

    // code not controlled by a lock

    unsigned val;

    if (val < 5) {
        // ... other thread can change val here ...
        switch (val) {
        case 0: // ...
        case 1: // ...
        case 2: // ...
        case 3: // ...
        case 4: // ...
        }
    }

Now, a compiler that does not know that `val` can change will  most likely implement that `switch` using a jump table with five entries.
Then, a `val` outside the `[0..4]` range will cause a jump to an address that could be anywhere in the program, and execution would proceed there.
Really, "all bets are off" if you get a data race.
Actually, it can be worse still: by looking at the generated code you may be able to determine where the stray jump will go for a given value;
this can be a security risk.

##### Enforcement

Some is possible, do at least something.
There are commercial and open-source tools that try to address this problem,
but be aware that solutions have costs and blind spots.
Static tools often have many false positives and run-time tools often have a significant cost.
We hope for better tools.
Using multiple tools can catch more problems than a single one.

There are other ways you can mitigate the chance of data races:

* Avoid global data
* Avoid `static` variables
* More use of value types on the stack (and don't pass pointers around too much)
* More use of immutable data (literals, `constexpr`, and `const`)

### <a name="Rconc-data"></a>CP.3: Minimize explicit sharing of writable data

##### Reason

If you don't share writable data, you can't have a data race.
The less sharing you do, the less chance you have to forget to synchronize access (and get data races).
The less sharing you do, the less chance you have to wait on a lock (so performance can improve).

##### Example

    bool validate(const vector<Reading>&);
    Graph<Temp_node> temperature_gradiants(const vector<Reading>&);
    Image altitude_map(const vector<Reading>&);
    // ...

    void process_readings(const vector<Reading>& surface_readings)
    {
        auto h1 = async([&] { if (!validate(surface_readings)) throw Invalid_data{}; });
        auto h2 = async([&] { return temperature_gradiants(surface_readings); });
        auto h3 = async([&] { return altitude_map(surface_readings); });
        // ...
        h1.get();
        auto v2 = h2.get();
        auto v3 = h3.get();
        // ...
    }

Without those `const`s, we would have to review every asynchronously invoked function for potential data races on `surface_readings`.
Making `surface_readings` be `const` (with respect to this function) allow reasoning using only the function body.

##### Note

Immutable data can be safely and efficiently shared.
No locking is needed: You can't have a data race on a constant.
See also [CP.mess: Message Passing](#SScp-mess) and [CP.31: prefer pass by value](#Rconc-data-by-value).

##### Enforcement

???


### <a name="Rconc-task"></a>CP.4: Think in terms of tasks, rather than threads

##### Reason

A `thread` is an implementation concept, a way of thinking about the machine.
A task is an application notion, something you'd like to do, preferably concurrently with other tasks.
Application concepts are easier to reason about.

##### Example

    void some_fun() {
        std::string msg, msg2;
        std::thread publisher([&] { msg = "Hello"; });       // bad: less expressive
                                                             //      and more error-prone
        auto pubtask = std::async([&] { msg2 = "Hello"; });  // OK
        // ...
        publisher.join();
    }

##### Note

With the exception of `async()`, the standard-library facilities are low-level, machine-oriented, threads-and-lock level.
This is a necessary foundation, but we have to try to raise the level of abstraction: for productivity, for reliability, and for performance.
This is a potent argument for using higher level, more applications-oriented libraries (if possibly, built on top of standard-library facilities).

##### Enforcement

???

### <a name="Rconc-volatile"></a>CP.8: Don't try to use `volatile` for synchronization

##### Reason

In C++, unlike some other languages, `volatile` does not provide atomicity, does not synchronize between threads,
and does not prevent instruction reordering (neither compiler nor hardware).
It simply has nothing to do with concurrency.

##### Example, bad:

    int free_slots = max_slots; // current source of memory for objects

    Pool* use()
    {
        if (int n = free_slots--) return &pool[n];
    }

Here we have a problem:
This is perfectly good code in a single-threaded program, but have two threads execute this and
there is a race condition on `free_slots` so that two threads might get the same value and `free_slots`.
That's (obviously) a bad data race, so people trained in other languages may try to fix it like this:

    volatile int free_slots = max_slots; // current source of memory for objects

    Pool* use()
    {
        if (int n = free_slots--) return &pool[n];
    }

This has no effect on synchronization: The data race is still there!

The C++ mechanism for this is `atomic` types:

    atomic<int> free_slots = max_slots; // current source of memory for objects

    Pool* use()
    {
        if (int n = free_slots--) return &pool[n];
    }

Now the `--` operation is atomic,
rather than a read-increment-write sequence where another thread might get in-between the individual operations.

##### Alternative

Use `atomic` types where you might have used `volatile` in some other language.
Use a `mutex` for more complicated examples.

##### See also

[(rare) proper uses of `volatile`](#Rconc-volatile2)

### <a name="Rconc-tools"></a>CP.9: Whenever feasible use tools to validate your concurrent code

Experience shows that concurrent code is exceptionally hard to get right
and that compile-time checking, run-time checks, and testing are less effective at finding concurrency errors
than they are at finding errors in sequential code.
Subtle concurrency errors can have dramatically bad effects, including memory corruption and deadlocks.

##### Example

    ???

##### Note

Thread safety is challenging, often getting the better of experienced programmers: tooling is an important strategy to mitigate those risks.
There are many tools "out there", both commercial and open-source tools, both research and production tools.
Unfortunately people's needs and constraints differ so dramatically that we cannot make specific recommendations,
but we can mention:

* Static enforcement tools: both [clang](http://clang.llvm.org/docs/ThreadSafetyAnalysis.html)
and some older versions of [GCC](https://gcc.gnu.org/wiki/ThreadSafetyAnnotation)
have some support for static annotation of thread safety properties.
Consistent use of this technique turns many classes of thread-safety errors into compile-time errors.
The annotations are generally local (marking a particular member variable as guarded by a particular mutex),
and are usually easy to learn. However, as with many static tools, it can often present false negatives;
cases that should have been caught but were allowed.

* dynamic enforcement tools: Clang's [Thread Sanitizer](http://clang.llvm.org/docs/ThreadSanitizer.html) (aka TSAN)
is a powerful example of dynamic tools: it changes the build and execution of your program to add bookkeeping on memory access,
absolutely identifying data races in a given execution of your binary.
The cost for this is both memory (5-10x in most cases) and CPU slowdown (2-20x).
Dynamic tools like this are best when applied to integration tests, canary pushes, or unittests that operate on multiple threads.
Workload matters: When TSAN identifies a problem, it is effectively always an actual data race,
but it can only identify races seen in a given execution.

##### Enforcement

It is up to an application builder to choose which support tools are valuable for a particular applications.

## <a name="SScp-con"></a>CP.con: Concurrency

This section focuses on relatively ad-hoc uses of multiple threads communicating through shared data.

* For parallel algorithms, see [parallelism](#SScp-par)
* For inter-task communication without explicit sharing, see [messaging](#SScp-mess)
* For vector parallel code, see [vectorization](#SScp-vec)
* For lock-free programming, see [lock free](#SScp-free)

Concurrency rule summary:

* [CP.20: Use RAII, never plain `lock()`/`unlock()`](#Rconc-raii)
* [CP.21: Use `std::lock()` or `std::scoped_lock` to acquire multiple `mutex`es](#Rconc-lock)
* [CP.22: Never call unknown code while holding a lock (e.g., a callback)](#Rconc-unknown)
* [CP.23: Think of a joining `thread` as a scoped container](#Rconc-join)
* [CP.24: Think of a `thread` as a global container](#Rconc-detach)
* [CP.25: Prefer `gsl::joining_thread` over `std::thread`](#Rconc-joining_thread)
* [CP.26: Don't `detach()` a thread](#Rconc-detached_thread)
* [CP.31: Pass small amounts of data between threads by value, rather than by reference or pointer](#Rconc-data-by-value)
* [CP.32: To share ownership between unrelated `thread`s use `shared_ptr`](#Rconc-shared)
* [CP.40: Minimize context switching](#Rconc-switch)
* [CP.41: Minimize thread creation and destruction](#Rconc-create)
* [CP.42: Don't `wait` without a condition](#Rconc-wait)
* [CP.43: Minimize time spent in a critical section](#Rconc-time)
* [CP.44: Remember to name your `lock_guard`s and `unique_lock`s](#Rconc-name)
* [CP.50: Define a `mutex` together with the data it guards. Use `synchronized_value<T>` where possible](#Rconc-mutex)
* ??? when to use a spinlock
* ??? when to use `try_lock()`
* ??? when to prefer `lock_guard` over `unique_lock`
* ??? Time multiplexing
* ??? when/how to use `new thread`

### <a name="Rconc-raii"></a>CP.20: Use RAII, never plain `lock()`/`unlock()`

##### Reason

Avoids nasty errors from unreleased locks.

##### Example, bad

    mutex mtx;

    void do_stuff()
    {
        mtx.lock();
        // ... do stuff ...
        mtx.unlock();
    }

Sooner or later, someone will forget the `mtx.unlock()`, place a `return` in the `... do stuff ...`, throw an exception, or something.

    mutex mtx;

    void do_stuff()
    {
        unique_lock<mutex> lck {mtx};
        // ... do stuff ...
    }

##### Enforcement

Flag calls of member `lock()` and `unlock()`.  ???


### <a name="Rconc-lock"></a>CP.21: Use `std::lock()` or `std::scoped_lock` to acquire multiple `mutex`es

##### Reason

To avoid deadlocks on multiple `mutex`es.

##### Example

This is asking for deadlock:

    // thread 1
    lock_guard<mutex> lck1(m1);
    lock_guard<mutex> lck2(m2);

    // thread 2
    lock_guard<mutex> lck2(m2);
    lock_guard<mutex> lck1(m1);

Instead, use `lock()`:

    // thread 1
    lock(m1, m2);
    lock_guard<mutex> lck1(m1, adopt_lock);
    lock_guard<mutex> lck2(m2, adopt_lock);

    // thread 2
    lock(m2, m1);
    lock_guard<mutex> lck2(m2, adopt_lock);
    lock_guard<mutex> lck1(m1, adopt_lock);

or (better, but C++17 only):

    // thread 1
    scoped_lock<mutex, mutex> lck1(m1, m2);

    // thread 2
    scoped_lock<mutex, mutex> lck2(m2, m1);

Here, the writers of `thread1` and `thread2` are still not agreeing on the order of the `mutex`es, but order no longer matters.

##### Note

In real code, `mutex`es are rarely named to conveniently remind the programmer of an intended relation and intended order of acquisition.
In real code, `mutex`es are not always conveniently acquired on consecutive lines.

In C++17 it's possible to write plain

    lock_guard lck1(m1, adopt_lock);

and have the `mutex` type deduced.

##### Enforcement

Detect the acquisition of multiple `mutex`es.
This is undecidable in general, but catching common simple examples (like the one above) is easy.


### <a name="Rconc-unknown"></a>CP.22: Never call unknown code while holding a lock (e.g., a callback)

##### Reason

If you don't know what a piece of code does, you are risking deadlock.

##### Example

    void do_this(Foo* p)
    {
        lock_guard<mutex> lck {my_mutex};
        // ... do something ...
        p->act(my_data);
        // ...
    }

If you don't know what `Foo::act` does (maybe it is a virtual function invoking a derived class member of a class not yet written),
it may call `do_this` (recursively) and cause a deadlock on `my_mutex`.
Maybe it will lock on a different mutex and not return in a reasonable time, causing delays to any code calling `do_this`.

##### Example

A common example of the "calling unknown code" problem is a call to a function that tries to gain locked access to the same object.
Such problem can often be solved by using a `recursive_mutex`. For example:

    recursive_mutex my_mutex;

    template<typename Action>
    void do_something(Action f)
    {
        unique_lock<recursive_mutex> lck {my_mutex};
        // ... do something ...
        f(this);    // f will do something to *this
        // ...
    }

If, as it is likely, `f()` invokes operations on `*this`, we must make sure that the object's invariant holds before the call.

##### Enforcement

* Flag calling a virtual function with a non-recursive `mutex` held
* Flag calling a callback with a non-recursive `mutex` held


### <a name="Rconc-join"></a>CP.23: Think of a joining `thread` as a scoped container

##### Reason

To maintain pointer safety and avoid leaks, we need to consider what pointers are used by a `thread`.
If a `thread` joins, we can safely pass pointers to objects in the scope of the `thread` and its enclosing scopes.

##### Example

    void f(int* p)
    {
        // ...
        *p = 99;
        // ...
    }
    int glob = 33;

    void some_fct(int* p)
    {
        int x = 77;
        joining_thread t0(f, &x);           // OK
        joining_thread t1(f, p);            // OK
        joining_thread t2(f, &glob);        // OK
        auto q = make_unique<int>(99);
        joining_thread t3(f, q.get());      // OK
        // ...
    }

A `gsl::joining_thread` is a `std::thread` with a destructor that joins and that cannot be `detached()`.
By "OK" we mean that the object will be in scope ("live") for as long as a `thread` can use the pointer to it.
The fact that `thread`s run concurrently doesn't affect the lifetime or ownership issues here;
these `thread`s can be seen as just a function object called from `some_fct`.

##### Enforcement

Ensure that `joining_thread`s don't `detach()`.
After that, the usual lifetime and ownership (for local objects) enforcement applies.

### <a name="Rconc-detach"></a>CP.24: Think of a `thread` as a global container

##### Reason

To maintain pointer safety and avoid leaks, we need to consider what pointers are used by a `thread`.
If a `thread` is detached, we can safely pass pointers to static and free store objects (only).

##### Example

    void f(int* p)
    {
        // ...
        *p = 99;
        // ...
    }

    int glob = 33;

    void some_fct(int* p)
    {
        int x = 77;
        std::thread t0(f, &x);           // bad
        std::thread t1(f, p);            // bad
        std::thread t2(f, &glob);        // OK
        auto q = make_unique<int>(99);
        std::thread t3(f, q.get());      // bad
        // ...
        t0.detach();
        t1.detach();
        t2.detach();
        t3.detach();
        // ...
    }

By "OK" we mean that the object will be in scope ("live") for as long as a `thread` can use the pointers to it.
By "bad" we mean that a `thread` may use a pointer after the pointed-to object is destroyed.
The fact that `thread`s run concurrently doesn't affect the lifetime or ownership issues here;
these `thread`s can be seen as just a function object called from `some_fct`.

##### Note

Even objects with static storage duration can be problematic if used from detached threads: if the
thread continues until the end of the program, it might be running concurrently with the destruction
of objects with static storage duration, and thus accesses to such objects might race.

##### Note

This rule is redundant if you [don't `detach()`](#Rconc-detached_thread) and [use `gsl::joining_thread`](#Rconc-joining_thread).
However, converting code to follow those guidelines could be difficult and even impossible for third-party libraries.
In such cases, the rule becomes essential for lifetime safety and type safety.


In general, it is undecidable whether a `detach()` is executed for a `thread`, but simple common cases are easily detected.
If we cannot prove that a `thread` does not `detach()`, we must assume that it does and that it outlives the scope in which it was constructed;
After that, the usual lifetime and ownership (for global objects) enforcement applies.

##### Enforcement

Flag attempts to pass local variables to a thread that might `detach()`.

### <a name="Rconc-joining_thread"></a>CP.25: Prefer `gsl::joining_thread` over `std::thread`

##### Reason

A `joining_thread` is a thread that joins at the end of its scope.
Detached threads are hard to monitor.
It is harder to ensure absence of errors in detached threads (and potentially detached threads)

##### Example, bad

    void f() { std::cout << "Hello "; }

    struct F {
        void operator()() { std::cout << "parallel world "; }
    };

    int main()
    {
        std::thread t1{f};      // f() executes in separate thread
        std::thread t2{F()};    // F()() executes in separate thread
    }  // spot the bugs

##### Example

    void f() { std::cout << "Hello "; }

    struct F {
        void operator()() { std::cout << "parallel world "; }
    };

    int main()
    {
        std::thread t1{f};      // f() executes in separate thread
        std::thread t2{F()};    // F()() executes in separate thread

        t1.join();
        t2.join();
    }  // one bad bug left


##### Example, bad

The code determining whether to `join()` or `detach()` may be complicated and even decided in the thread of functions called from it or functions called by the function that creates a thread:

    void tricky(thread* t, int n)
    {
        // ...
        if (is_odd(n))
            t->detach();
        // ...
    }

    void use(int n)
    {
        thread t { tricky, this, n };
        // ...
        // ... should I join here? ...
    }

This seriously complicates lifetime analysis, and in not too unlikely cases makes lifetime analysis impossible.
This implies that we cannot safely refer to local objects in `use()` from the thread or refer to local objects in the thread from `use()`.

##### Note

Make "immortal threads" globals, put them in an enclosing scope, or put them on the free store rather than `detach()`.
[don't `detach`](#Rconc-detached_thread).

##### Note

Because of old code and third party libraries using `std::thread` this rule can be hard to introduce.

##### Enforcement

Flag uses of `std::thread`:

* Suggest use of `gsl::joining_thread`.
* Suggest ["exporting ownership"](#Rconc-detached_thread) to an enclosing scope if it detaches.
* Seriously warn if it is not obvious whether if joins of detaches.

### <a name="Rconc-detached_thread"></a>CP.26: Don't `detach()` a thread

##### Reason

Often, the need to outlive the scope of its creation is inherent in the `thread`s task,
but implementing that idea by `detach` makes it harder to monitor and communicate with the detached thread.
In particular, it is harder (though not impossible) to ensure that the thread completed as expected or lives for as long as expected.

##### Example

    void heartbeat();

    void use()
    {
        std::thread t(heartbeat);             // don't join; heartbeat is meant to run forever
        t.detach();
        // ...
    }

This is a reasonable use of a thread, for which `detach()` is commonly used.
There are problems, though.
How do we monitor the detached thread to see if it is alive?
Something might go wrong with the heartbeat, and losing a heartbeat can be very serious in a system for which it is needed.
So, we need to communicate with the heartbeat thread
(e.g., through a stream of messages or notification events using a `condition_variable`).

An alternative, and usually superior solution is to control its lifetime by placing it in a scope outside its point of creation (or activation).
For example:

    void heartbeat();

    gsl::joining_thread t(heartbeat);             // heartbeat is meant to run "forever"

This heartbeat will (barring error, hardware problems, etc.) run for as long as the program does.

Sometimes, we need to separate the point of creation from the point of ownership:

    void heartbeat();

    unique_ptr<gsl::joining_thread> tick_tock {nullptr};

    void use()
    {
        // heartbeat is meant to run as long as tick_tock lives
        tick_tock = make_unique<gsl::joining_thread>(heartbeat);
        // ...
    }

#### Enforcement

Flag `detach()`.


### <a name="Rconc-data-by-value"></a>CP.31: Pass small amounts of data between threads by value, rather than by reference or pointer

##### Reason

Copying a small amount of data is cheaper to copy and access than to share it using some locking mechanism.
Copying naturally gives unique ownership (simplifies code) and eliminates the possibility of data races.

##### Note

Defining "small amount" precisely is impossible.

##### Example

    string modify1(string);
    void modify2(string&);

    void fct(string& s)
    {
        auto res = async(modify1, s);
        async(modify2, s);
    }

The call of `modify1` involves copying two `string` values; the call of `modify2` does not.
On the other hand, the implementation of `modify1` is exactly as we would have written it for single-threaded code,
whereas the implementation of `modify2` will need some form of locking to avoid data races.
If the string is short (say 10 characters), the call of `modify1` can be surprisingly fast;
essentially all the cost is in the `thread` switch. If the string is long (say 1,000,000 characters), copying it twice
is probably not a good idea.

Note that this argument has nothing to do with `async` as such. It applies equally to considerations about whether to use
message passing or shared memory.

##### Enforcement

???


### <a name="Rconc-shared"></a>CP.32: To share ownership between unrelated `thread`s use `shared_ptr`

##### Reason

If threads are unrelated (that is, not known to be in the same scope or one within the lifetime of the other)
and they need to share free store memory that needs to be deleted, a `shared_ptr` (or equivalent) is the only
safe way to ensure proper deletion.

##### Example

    ???

##### Note

* A static object (e.g. a global) can be shared because it is not owned in the sense that some thread is responsible for its deletion.
* An object on free store that is never to be deleted can be shared.
* An object owned by one thread can be safely shared with another as long as that second thread doesn't outlive the owner.

##### Enforcement

???


### <a name="Rconc-switch"></a>CP.40: Minimize context switching

##### Reason

Context switches are expensive.

##### Example

    ???

##### Enforcement

???


### <a name="Rconc-create"></a>CP.41: Minimize thread creation and destruction

##### Reason

Thread creation is expensive.

##### Example

    void worker(Message m)
    {
        // process
    }

    void master(istream& is)
    {
        for (Message m; is >> m; )
            run_list.push_back(new thread(worker, m));
    }

This spawns a `thread` per message, and the `run_list` is presumably managed to destroy those tasks once they are finished.

Instead, we could have a set of pre-created worker threads processing the messages

    Sync_queue<Message> work;

    void master(istream& is)
    {
        for (Message m; is >> m; )
            work.put(m);
    }

    void worker()
    {
        for (Message m; m = work.get(); ) {
            // process
        }
    }

    void workers()  // set up worker threads (specifically 4 worker threads)
    {
        joining_thread w1 {worker};
        joining_thread w2 {worker};
        joining_thread w3 {worker};
        joining_thread w4 {worker};
    }

##### Note

If your system has a good thread pool, use it.
If your system has a good message queue, use it.

##### Enforcement

???


### <a name="Rconc-wait"></a>CP.42: Don't `wait` without a condition

##### Reason

A `wait` without a condition can miss a wakeup or wake up simply to find that there is no work to do.

##### Example, bad

    std::condition_variable cv;
    std::mutex mx;

    void thread1()
    {
        while (true) {
            // do some work ...
            std::unique_lock<std::mutex> lock(mx);
            cv.notify_one();    // wake other thread
        }
    }

    void thread2()
    {
        while (true) {
            std::unique_lock<std::mutex> lock(mx);
            cv.wait(lock);    // might block forever
            // do work ...
        }
    }

Here, if some other `thread` consumes `thread1`'s notification, `thread2` can wait forever.

##### Example

    template<typename T>
    class Sync_queue {
    public:
        void put(const T& val);
        void put(T&& val);
        void get(T& val);
    private:
        mutex mtx;
        condition_variable cond;    // this controls access
        list<T> q;
    };

    template<typename T>
    void Sync_queue<T>::put(const T& val)
    {
        lock_guard<mutex> lck(mtx);
        q.push_back(val);
        cond.notify_one();
    }

    template<typename T>
    void Sync_queue<T>::get(T& val)
    {
        unique_lock<mutex> lck(mtx);
        cond.wait(lck, [this]{ return !q.empty(); });    // prevent spurious wakeup
        val = q.front();
        q.pop_front();
    }

Now if the queue is empty when a thread executing `get()` wakes up (e.g., because another thread has gotten to `get()` before it),
it will immediately go back to sleep, waiting.

##### Enforcement

Flag all `wait`s without conditions.


### <a name="Rconc-time"></a>CP.43: Minimize time spent in a critical section

##### Reason

The less time is spent with a `mutex` taken, the less chance that another `thread` has to wait,
and `thread` suspension and resumption are expensive.

##### Example

    void do_something() // bad
    {
        unique_lock<mutex> lck(my_lock);
        do0();  // preparation: does not need lock
        do1();  // transaction: needs locking
        do2();  // cleanup: does not need locking
    }

Here, we are holding the lock for longer than necessary:
We should not have taken the lock before we needed it and should have released it again before starting the cleanup.
We could rewrite this to

    void do_something() // bad
    {
        do0();  // preparation: does not need lock
        my_lock.lock();
        do1();  // transaction: needs locking
        my_lock.unlock();
        do2();  // cleanup: does not need locking
    }

But that compromises safety and violates the [use RAII](#Rconc-raii) rule.
Instead, add a block for the critical section:

    void do_something() // OK
    {
        do0();  // preparation: does not need lock
        {
            unique_lock<mutex> lck(my_lock);
            do1();  // transaction: needs locking
        }
        do2();  // cleanup: does not need locking
    }

##### Enforcement

Impossible in general.
Flag "naked" `lock()` and `unlock()`.


### <a name="Rconc-name"></a>CP.44: Remember to name your `lock_guard`s and `unique_lock`s

##### Reason

An unnamed local objects is a temporary that immediately goes out of scope.

##### Example

    unique_lock<mutex>(m1);
    lock_guard<mutex> {m2};
    lock(m1, m2);

This looks innocent enough, but it isn't.

##### Enforcement

Flag all unnamed `lock_guard`s and `unique_lock`s.



### <a name="Rconc-mutex"></a>CP.50: Define a `mutex` together with the data it guards. Use `synchronized_value<T>` where possible

##### Reason

It should be obvious to a reader that the data is to be guarded and how. This decreases the chance of the wrong mutex being locked, or the mutex not being locked.

Using a `synchronized_value<T>` ensures that the data has a mutex, and the right mutex is locked when the data is accessed.
See the [WG21 proposal](http://wg21.link/p0290) to add `synchronized_value` to a future TS or revision of the C++ standard.

##### Example

    struct Record {
        std::mutex m;   // take this mutex before accessing other members
        // ...
    };

    class MyClass {
        struct DataRecord {
           // ...
        };
        synchronized_value<DataRecord> data; // Protect the data with a mutex
    };

##### Enforcement

??? Possible?


## <a name="SScp-par"></a>CP.par: Parallelism

By "parallelism" we refer to performing a task (more or less) simultaneously ("in parallel with") on many data items.

Parallelism rule summary:

* ???
* ???
* Where appropriate, prefer the standard-library parallel algorithms
* Use algorithms that are designed for parallelism, not algorithms with unnecessary dependency on linear evaluation



## <a name="SScp-mess"></a>CP.mess: Message passing

The standard-library facilities are quite low-level, focused on the needs of close-to the hardware critical programming using `thread`s, `mutex`es, `atomic` types, etc.
Most people shouldn't work at this level: it's error-prone and development is slow.
If possible, use a higher level facility: messaging libraries, parallel algorithms, and vectorization.
This section looks at passing messages so that a programmer doesn't have to do explicit synchronization.

Message passing rules summary:

* [CP.60: Use a `future` to return a value from a concurrent task](#Rconc-future)
* [CP.61: Use an `async()` to spawn a concurrent task](#Rconc-async)
* message queues
* messaging libraries

???? should there be a "use X rather than `std::async`" where X is something that would use a better specified thread pool?

??? Is `std::async` worth using in light of future (and even existing, as libraries) parallelism facilities? What should the guidelines recommend if someone wants to parallelize, e.g., `std::accumulate` (with the additional precondition of commutativity), or merge sort?


### <a name="Rconc-future"></a>CP.60: Use a `future` to return a value from a concurrent task

##### Reason

A `future` preserves the usual function call return semantics for asynchronous tasks.
There is no explicit locking and both correct (value) return and error (exception) return are handled simply.

##### Example

    ???

##### Note

???

##### Enforcement

???

### <a name="Rconc-async"></a>CP.61: Use an `async()` to spawn a concurrent task

##### Reason

A `future` preserves the usual function call return semantics for asynchronous tasks.
There is no explicit locking and both correct (value) return and error (exception) return are handled simply.

##### Example

    ???

##### Note

Unfortunately, `async()` is not perfect.
For example, there is no guarantee that a thread pool is used to minimize thread construction.
In fact, most current `async()` implementations don't.
However, `async()` is simple and logically correct so until something better comes along
and unless you really need to optimize for many asynchronous tasks, stick with `async()`.

##### Enforcement

???


## <a name="SScp-vec"></a>CP.vec: Vectorization

Vectorization is a technique for executing a number of tasks concurrently without introducing explicit synchronization.
An operation is simply applied to elements of a data structure (a vector, an array, etc.) in parallel.
Vectorization has the interesting property of often requiring no non-local changes to a program.
However, vectorization works best with simple data structures and with algorithms specifically crafted to enable it.

Vectorization rule summary:

* ???
* ???

## <a name="SScp-free"></a>CP.free: Lock-free programming

Synchronization using `mutex`es and `condition_variable`s can be relatively expensive.
Furthermore, it can lead to deadlock.
For performance and to eliminate the possibility of deadlock, we sometimes have to use the tricky low-level "lock-free" facilities
that rely on briefly gaining exclusive ("atomic") access to memory.
Lock-free programming is also used to implement higher-level concurrency mechanisms, such as `thread`s and `mutex`es.

Lock-free programming rule summary:

* [CP.100: Don't use lock-free programming unless you absolutely have to](#Rconc-lockfree)
* [CP.101: Distrust your hardware/compiler combination](#Rconc-distrust)
* [CP.102: Carefully study the literature](#Rconc-literature)
* how/when to use atomics
* avoid starvation
* use a lock-free data structure rather than hand-crafting specific lock-free access
* [CP.110: Do not write your own double-checked locking for initialization](#Rconc-double)
* [CP.111: Use a conventional pattern if you really need double-checked locking](#Rconc-double-pattern)
* how/when to compare and swap


### <a name="Rconc-lockfree"></a>CP.100: Don't use lock-free programming unless you absolutely have to

##### Reason

It's error-prone and requires expert level knowledge of language features, machine architecture, and data structures.

##### Example, bad

    extern atomic<Link*> head;        // the shared head of a linked list

    Link* nh = new Link(data, nullptr);    // make a link ready for insertion
    Link* h = head.load();                 // read the shared head of the list

    do {
        if (h->data <= data) break;        // if so, insert elsewhere
        nh->next = h;                      // next element is the previous head
    } while (!head.compare_exchange_weak(h, nh));    // write nh to head or to h

Spot the bug.
It would be really hard to find through testing.
Read up on the ABA problem.

##### Exception

[Atomic variables](#???) can be used simply and safely, as long as you are using the sequentially consistent memory model (memory_order_seq_cst), which is the default.

##### Note

Higher-level concurrency mechanisms, such as `thread`s and `mutex`es are implemented using lock-free programming.

**Alternative**: Use lock-free data structures implemented by others as part of some library.


### <a name="Rconc-distrust"></a>CP.101: Distrust your hardware/compiler combination

##### Reason

The low-level hardware interfaces used by lock-free programming are among the hardest to implement well and among
the areas where the most subtle portability problems occur.
If you are doing lock-free programming for performance, you need to check for regressions.

##### Note

Instruction reordering (static and dynamic) makes it hard for us to think effectively at this level (especially if you use relaxed memory models).
Experience, (semi)formal models and model checking can be useful.
Testing - often to an extreme extent - is essential.
"Don't fly too close to the sun."

##### Enforcement

Have strong rules for re-testing in place that covers any change in hardware, operating system, compiler, and libraries.


### <a name="Rconc-literature"></a>CP.102: Carefully study the literature

##### Reason

With the exception of atomics and a few use standard patterns, lock-free programming is really an expert-only topic.
Become an expert before shipping lock-free code for others to use.

##### References

* Anthony Williams: C++ concurrency in action. Manning Publications.
* Boehm, Adve, You Don't Know Jack About Shared Variables or Memory Models , Communications of the ACM, Feb 2012.
* Boehm, "Threads Basics", HPL TR 2009-259.
* Adve, Boehm, "Memory Models: A Case for Rethinking Parallel Languages and Hardware", Communications of the ACM, August 2010.
* Boehm, Adve, "Foundations of the C++ Concurrency Memory Model", PLDI 08.
* Mark Batty, Scott Owens, Susmit Sarkar, Peter Sewell, and Tjark Weber, "Mathematizing C++ Concurrency", POPL 2011.
* Damian Dechev, Peter Pirkelbauer, and Bjarne Stroustrup: Understanding and Effectively Preventing the ABA Problem in Descriptor-based Lock-free Designs. 13th IEEE Computer Society ISORC 2010 Symposium. May 2010.
* Damian Dechev and Bjarne Stroustrup: Scalable Non-blocking Concurrent Objects for Mission Critical Code. ACM OOPSLA'09. October 2009
* Damian Dechev, Peter Pirkelbauer, Nicolas Rouquette, and Bjarne Stroustrup: Semantically Enhanced Containers for Concurrent Real-Time Systems. Proc. 16th Annual IEEE International Conference and Workshop on the Engineering of Computer Based Systems (IEEE ECBS). April 2009.


### <a name="Rconc-double"></a>CP.110: Do not write your own double-checked locking for initialization

##### Reason

Since C++11, static local variables are now initialized in a thread-safe way. When combined with the RAII pattern, static local variables can replace the need for writing your own double-checked locking for initialization. std::call_once can also achieve the same purpose. Use either static local variables of C++11 or std::call_once instead of writing your own double-checked locking for initialization.

##### Example

Example with std::call_once.

    void f()
    {
        static std::once_flag my_once_flag;
        std::call_once(my_once_flag, []()
        {
            // do this only once
        });
        // ...
    }

Example with thread-safe static local variables of C++11.

    void f()
    {
        // Assuming the compiler is compliant with C++11
        static My_class my_object; // Constructor called only once
        // ...
    }

    class My_class
    {
    public:
        My_class()
        {
            // do this only once
        }
    };

##### Enforcement

??? Is it possible to detect the idiom?


### <a name="Rconc-double-pattern"></a>CP.111: Use a conventional pattern if you really need double-checked locking

##### Reason

Double-checked locking is easy to mess up. If you really need to write your own double-checked locking, in spite of the rules [CP.110: Do not write your own double-checked locking for initialization](#Rconc-double) and [CP.100: Don't use lock-free programming unless you absolutely have to](#Rconc-lockfree), then do it in a conventional pattern.

The uses of the double-checked locking pattern that are not in violation of [CP.110: Do not write your own double-checked locking for initialization](#Rconc-double) arise when a non-thread-safe action is both hard and rare, and there exists a fast thread-safe test that can be used to guarantee that the action is not needed, but cannot be used to guarantee the converse.

##### Example, bad

The use of volatile does not make the first check thread-safe, see also [CP.200: Use `volatile` only to talk to non-C++ memory](#Rconc-volatile2)

    mutex action_mutex;
    volatile bool action_needed;

    if (action_needed) {
        std::lock_guard<std::mutex> lock(action_mutex);
        if (action_needed) {
            take_action();
            action_needed = false;
        }
    }

##### Example, good

    mutex action_mutex;
    atomic<bool> action_needed;

    if (action_needed) {
        std::lock_guard<std::mutex> lock(action_mutex);
        if (action_needed) {
            take_action();
            action_needed = false;
        }
    }

Fine-tuned memory order may be beneficial where acquire load is more efficient than sequentially-consistent load

    mutex action_mutex;
    atomic<bool> action_needed;

    if (action_needed.load(memory_order_acquire)) {
        lock_guard<std::mutex> lock(action_mutex);
        if (action_needed.load(memory_order_relaxed)) {
            take_action();
            action_needed.store(false, memory_order_release);
        }
    }

##### Enforcement

??? Is it possible to detect the idiom?


## <a name="SScp-etc"></a>CP.etc: Etc. concurrency rules

These rules defy simple categorization:

* [CP.200: Use `volatile` only to talk to non-C++ memory](#Rconc-volatile2)
* [CP.201: ??? Signals](#Rconc-signal)

### <a name="Rconc-volatile2"></a>CP.200: Use `volatile` only to talk to non-C++ memory

##### Reason

`volatile` is used to refer to objects that are shared with "non-C++" code or hardware that does not follow the C++ memory model.

##### Example

    const volatile long clock;

This describes a register constantly updated by a clock circuit.
`clock` is `volatile` because its value will change without any action from the C++ program that uses it.
For example, reading `clock` twice will often yield two different values, so the optimizer had better not optimize away the second read in this code:

    long t1 = clock;
    // ... no use of clock here ...
    long t2 = clock;

`clock` is `const` because the program should not try to write to `clock`.

##### Note

Unless you are writing the lowest level code manipulating hardware directly, consider `volatile` an esoteric feature that is best avoided.

##### Example

Usually C++ code receives `volatile` memory that is owned elsewhere (hardware or another language):

    int volatile* vi = get_hardware_memory_location();
        // note: we get a pointer to someone else's memory here
        // volatile says "treat this with extra respect"

Sometimes C++ code allocates the `volatile` memory and shares it with "elsewhere" (hardware or another language) by deliberately escaping a pointer:

    static volatile long vl;
    please_use_this(&vl);   // escape a reference to this to "elsewhere" (not C++)

##### Example, bad

`volatile` local variables are nearly always wrong -- how can they be shared with other languages or hardware if they're ephemeral?
The same applies almost as strongly to member variables, for the same reason.

    void f() {
        volatile int i = 0; // bad, volatile local variable
        // etc.
    }

    class My_type {
        volatile int i = 0; // suspicious, volatile member variable
        // etc.
    };

##### Note

In C++, unlike in some other languages, `volatile` has [nothing to do with synchronization](#Rconc-volatile).

##### Enforcement

* Flag `volatile T` local and member variables; almost certainly you intended to use `atomic<T>` instead.
* ???

### <a name="Rconc-signal"></a>CP.201: ??? Signals

???UNIX signal handling???. May be worth reminding how little is async-signal-safe, and how to communicate with a signal handler (best is probably "not at all")


# <a name="S-errors"></a>E: Error handling

Error handling involves:

* Detecting an error
* Transmitting information about an error to some handler code
* Preserving a valid state of the program
* Avoiding resource leaks

It is not possible to recover from all errors. If recovery from an error is not possible, it is important to quickly "get out" in a well-defined way. A strategy for error handling must be simple, or it becomes a source of even worse errors.  Untested and rarely executed error-handling code is itself the source of many bugs.

The rules are designed to help avoid several kinds of errors:

* Type violations (e.g., misuse of `union`s and casts)
* Resource leaks (including memory leaks)
* Bounds errors
* Lifetime errors (e.g., accessing an object after is has been `delete`d)
* Complexity errors (logical errors made likely by overly complex expression of ideas)
* Interface errors (e.g., an unexpected value is passed through an interface)

Error-handling rule summary:

* [E.1: Develop an error-handling strategy early in a design](#Re-design)
* [E.2: Throw an exception to signal that a function can't perform its assigned task](#Re-throw)
* [E.3: Use exceptions for error handling only](#Re-errors)
* [E.4: Design your error-handling strategy around invariants](#Re-design-invariants)
* [E.5: Let a constructor establish an invariant, and throw if it cannot](#Re-invariant)
* [E.6: Use RAII to prevent leaks](#Re-raii)
* [E.7: State your preconditions](#Re-precondition)
* [E.8: State your postconditions](#Re-postcondition)

* [E.12: Use `noexcept` when exiting a function because of a `throw` is impossible or unacceptable](#Re-noexcept)
* [E.13: Never throw while being the direct owner of an object](#Re-never-throw)
* [E.14: Use purpose-designed user-defined types as exceptions (not built-in types)](#Re-exception-types)
* [E.15: Catch exceptions from a hierarchy by reference](#Re-exception-ref)
* [E.16: Destructors, deallocation, and `swap` must never fail](#Re-never-fail)
* [E.17: Don't try to catch every exception in every function](#Re-not-always)
* [E.18: Minimize the use of explicit `try`/`catch`](#Re-catch)
* [E.19: Use a `final_action` object to express cleanup if no suitable resource handle is available](#Re-finally)

* [E.25: If you can't throw exceptions, simulate RAII for resource management](#Re-no-throw-raii)
* [E.26: If you can't throw exceptions, consider failing fast](#Re-no-throw-crash)
* [E.27: If you can't throw exceptions, use error codes systematically](#Re-no-throw-codes)
* [E.28: Avoid error handling based on global state (e.g. `errno`)](#Re-no-throw)

* [E.30: Don't use exception specifications](#Re-specifications)
* [E.31: Properly order your `catch`-clauses](#Re_catch)

### <a name="Re-design"></a>E.1: Develop an error-handling strategy early in a design

##### Reason

A consistent and complete strategy for handling errors and resource leaks is hard to retrofit into a system.

### <a name="Re-throw"></a>E.2: Throw an exception to signal that a function can't perform its assigned task

##### Reason

To make error handling systematic, robust, and non-repetitive.

##### Example

    struct Foo {
        vector<Thing> v;
        File_handle f;
        string s;
    };

    void use()
    {
        Foo bar {{Thing{1}, Thing{2}, Thing{monkey}}, {"my_file", "r"}, "Here we go!"};
        // ...
    }

Here, `vector` and `string`s constructors may not be able to allocate sufficient memory for their elements, `vector`s constructor may not be able copy the `Thing`s in its initializer list, and `File_handle` may not be able to open the required file.
In each case, they throw an exception for `use()`'s caller to handle.
If `use()` could handle the failure to construct `bar` it can take control using `try`/`catch`.
In either case, `Foo`'s constructor correctly destroys constructed members before passing control to whatever tried to create a `Foo`.
Note that there is no return value that could contain an error code.

The `File_handle` constructor might be defined like this:

    File_handle::File_handle(const string& name, const string& mode)
        :f{fopen(name.c_str(), mode.c_str())}
    {
        if (!f)
            throw runtime_error{"File_handle: could not open " + name + " as " + mode};
    }

##### Note

It is often said that exceptions are meant to signal exceptional events and failures.
However, that's a bit circular because "what is exceptional?"
Examples:

* A precondition that cannot be met
* A constructor that cannot construct an object (failure to establish its class's [invariant](#Rc-struct))
* An out-of-range error (e.g., `v[v.size()] = 7`)
* Inability to acquire a resource (e.g., the network is down)

In contrast, termination of an ordinary loop is not exceptional.
Unless the loop was meant to be infinite, termination is normal and expected.

##### Note

Don't use a `throw` as simply an alternative way of returning a value from a function.

##### Exception

Some systems, such as hard-real-time systems require a guarantee that an action is taken in a (typically short) constant maximum time known before execution starts. Such systems can use exceptions only if there is tool support for accurately predicting the maximum time to recover from a `throw`.

**See also**: [RAII](#Re-raii)

**See also**: [discussion](#Sd-noexcept)

##### Note

Before deciding that you cannot afford or don't like exception-based error handling, have a look at the [alternatives](#Re-no-throw-raii);
they have their own complexities and problems.
Also, as far as possible, measure before making claims about efficiency.

### <a name="Re-errors"></a>E.3: Use exceptions for error handling only

##### Reason

To keep error handling separated from "ordinary code."
C++ implementations tend to be optimized based on the assumption that exceptions are rare.

##### Example, don't

    // don't: exception not used for error handling
    int find_index(vector<string>& vec, const string& x)
    {
        try {
            for (gsl::index i = 0; i < vec.size(); ++i)
                if (vec[i] == x) throw i;  // found x
        } catch (int i) {
            return i;
        }
        return -1;   // not found
    }

This is more complicated and most likely runs much slower than the obvious alternative.
There is nothing exceptional about finding a value in a `vector`.

##### Enforcement

Would need to be heuristic.
Look for exception values "leaked" out of `catch` clauses.

### <a name="Re-design-invariants"></a>E.4: Design your error-handling strategy around invariants

##### Reason

To use an object it must be in a valid state (defined formally or informally by an invariant) and to recover from an error every object not destroyed must be in a valid state.

##### Note

An [invariant](#Rc-struct) is a logical condition for the members of an object that a constructor must establish for the public member functions to assume.

##### Enforcement

???

### <a name="Re-invariant"></a>E.5: Let a constructor establish an invariant, and throw if it cannot

##### Reason

Leaving an object without its invariant established is asking for trouble.
Not all member functions can be called.

##### Example

    class Vector {  // very simplified vector of doubles
        // if elem != nullptr then elem points to sz doubles
    public:
        Vector() : elem{nullptr}, sz{0}{}
        Vector(int s) : elem{new double[s]}, sz{s} { /* initialize elements */ }
        ~Vector() { delete [] elem; }
        double& operator[](int s) { return elem[s]; }
        // ...
    private:
        owner<double*> elem;
        int sz;
    };

The class invariant - here stated as a comment - is established by the constructors.
`new` throws if it cannot allocate the required memory.
The operators, notably the subscript operator, relies on the invariant.

**See also**: [抛出异常，如果构造函数不能构造出合法的对象](#Rc-throw)

##### Enforcement

Flag classes with `private` state without a constructor (public, protected, or private).

### <a name="Re-raii"></a>E.6: Use RAII to prevent leaks

##### Reason

Leaks are typically unacceptable.
Manual resource release is error-prone.
RAII ("Resource Acquisition Is Initialization") is the simplest, most systematic way of preventing leaks.

##### Example

    void f1(int i)   // Bad: possibly leak
    {
        int* p = new int[12];
        // ...
        if (i < 17) throw Bad{"in f()", i};
        // ...
    }

We could carefully release the resource before the throw:

    void f2(int i)   // Clumsy and error-prone: explicit release
    {
        int* p = new int[12];
        // ...
        if (i < 17) {
            delete[] p;
            throw Bad{"in f()", i};
        }
        // ...
    }

This is verbose. In larger code with multiple possible `throw`s explicit releases become repetitive and error-prone.

    void f3(int i)   // OK: resource management done by a handle (but see below)
    {
        auto p = make_unique<int[]>(12);
        // ...
        if (i < 17) throw Bad{"in f()", i};
        // ...
    }

Note that this works even when the `throw` is implicit because it happened in a called function:

    void f4(int i)   // OK: resource management done by a handle (but see below)
    {
        auto p = make_unique<int[]>(12);
        // ...
        helper(i);   // may throw
        // ...
    }

Unless you really need pointer semantics, use a local resource object:

    void f5(int i)   // OK: resource management done by local object
    {
        vector<int> v(12);
        // ...
        helper(i);   // may throw
        // ...
    }

That's even simpler and safer, and often more efficient.

##### Note

If there is no obvious resource handle and for some reason defining a proper RAII object/handle is infeasible,
as a last resort, cleanup actions can be represented by a [`final_action`](#Re-finally) object.

##### Note

But what do we do if we are writing a program where exceptions cannot be used?
First challenge that assumption; there are many anti-exceptions myths around.
We know of only a few good reasons:

* We are on a system so small that the exception support would eat up most of our 2K memory.
* We are in a hard-real-time system and we don't have tools that guarantee us that an exception is handled within the required time.
* We are in a system with tons of legacy code using lots of pointers in difficult-to-understand ways
  (in particular without a recognizable ownership strategy) so that exceptions could cause leaks.
* Our implementation of the C++ exception mechanisms is unreasonably poor
(slow, memory consuming, failing to work correctly for dynamically linked libraries, etc.).
Complain to your implementation purveyor; if no user complains, no improvement will happen.
* We get fired if we challenge our manager's ancient wisdom.

Only the first of these reasons is fundamental, so whenever possible, use exceptions to implement RAII, or design your RAII objects to never fail.
When exceptions cannot be used, simulate RAII.
That is, systematically check that objects are valid after construction and still release all resources in the destructor.
One strategy is to add a `valid()` operation to every resource handle:

    void f()
    {
        vector<string> vs(100);   // not std::vector: valid() added
        if (!vs.valid()) {
            // handle error or exit
        }

        ifstream fs("foo");   // not std::ifstream: valid() added
        if (!fs.valid()) {
            // handle error or exit
        }

        // ...
    } // destructors clean up as usual

Obviously, this increases the size of the code, doesn't allow for implicit propagation of "exceptions" (`valid()` checks), and `valid()` checks can be forgotten.
Prefer to use exceptions.

**See also**: [Use of `noexcept`](#Se-noexcept)

##### Enforcement

???

### <a name="Re-precondition"></a>E.7: State your preconditions

##### Reason

To avoid interface errors.

**See also**: [precondition rule](#Ri-pre)

### <a name="Re-postcondition"></a>E.8: State your postconditions

##### Reason

To avoid interface errors.

**See also**: [postcondition rule](#Ri-post)

### <a name="Re-noexcept"></a>E.12: Use `noexcept` when exiting a function because of a `throw` is impossible or unacceptable

##### Reason

To make error handling systematic, robust, and efficient.

##### Example

    double compute(double d) noexcept
    {
        return log(sqrt(d <= 0 ? 1 : d));
    }

Here, we know that `compute` will not throw because it is composed out of operations that don't throw.
By declaring `compute` to be `noexcept`, we give the compiler and human readers information that can make it easier for them to understand and manipulate `compute`.

##### Note

Many standard-library functions are `noexcept` including all the standard-library functions "inherited" from the C Standard Library.

##### Example

    vector<double> munge(const vector<double>& v) noexcept
    {
        vector<double> v2(v.size());
        // ... do something ...
    }

The `noexcept` here states that I am not willing or able to handle the situation where I cannot construct the local `vector`.
That is, I consider memory exhaustion a serious design error (on par with hardware failures) so that I'm willing to crash the program if it happens.

##### Note

Do not use traditional [exception-specifications](#Re-specifications).

##### See also

[discussion](#Sd-noexcept).

### <a name="Re-never-throw"></a>E.13: Never throw while being the direct owner of an object

##### Reason

That would be a leak.

##### Example

    void leak(int x)   // don't: may leak
    {
        auto p = new int{7};
        if (x < 0) throw Get_me_out_of_here{};  // may leak *p
        // ...
        delete p;   // we may never get here
    }

One way of avoiding such problems is to use resource handles consistently:

    void no_leak(int x)
    {
        auto p = make_unique<int>(7);
        if (x < 0) throw Get_me_out_of_here{};  // will delete *p if necessary
        // ...
        // no need for delete p
    }

Another solution (often better) would be to use a local variable to eliminate explicit use of pointers:

    void no_leak_simplified(int x)
    {
        vector<int> v(7);
        // ...
    }

##### Note

If you have local "things" that requires cleanup, but is not represented by an object with a destructor, such cleanup must
also be done before a `throw`.
Sometimes, [`finally()`](#Re-finally) can make such unsystematic cleanup a bit more manageable.

### <a name="Re-exception-types"></a>E.14: Use purpose-designed user-defined types as exceptions (not built-in types)

##### Reason

A user-defined type is unlikely to clash with other people's exceptions.

##### Example

    void my_code()
    {
        // ...
        throw Moonphase_error{};
        // ...
    }

    void your_code()
    {
        try {
            // ...
            my_code();
            // ...
        }
        catch(const Bufferpool_exhausted&) {
            // ...
        }
    }

##### Example, don't

    void my_code()     // Don't
    {
        // ...
        throw 7;       // 7 means "moon in the 4th quarter"
        // ...
    }

    void your_code()   // Don't
    {
        try {
            // ...
            my_code();
            // ...
        }
        catch(int i) {  // i == 7 means "input buffer too small"
            // ...
        }
    }

##### Note

The standard-library classes derived from `exception` should be used only as base classes or for exceptions that require only "generic" handling. Like built-in types, their use could clash with other people's use of them.

##### Example, don't

    void my_code()   // Don't
    {
        // ...
        throw runtime_error{"moon in the 4th quarter"};
        // ...
    }

    void your_code()   // Don't
    {
        try {
            // ...
            my_code();
            // ...
        }
        catch(const runtime_error&) {   // runtime_error means "input buffer too small"
            // ...
        }
    }

**See also**: [Discussion](#Sd-???)

##### Enforcement

Catch `throw` and `catch` of a built-in type. Maybe warn about `throw` and `catch` using a standard-library `exception` type. Obviously, exceptions derived from the `std::exception` hierarchy are fine.

### <a name="Re-exception-ref"></a>E.15: Catch exceptions from a hierarchy by reference

##### Reason

To prevent slicing.

##### Example

    void f()
    {
        try {
            // ...
        }
        catch (exception e) {   // don't: may slice
            // ...
        }
    }

Instead, use a reference:

    catch (exception& e) { /* ... */ }

of - typically better still - a `const` reference:

    catch (const exception& e) { /* ... */ }

Most handlers do not modify their exception and in general we [recommend use of `const`](#Res-const).

##### Note

To rethrow a caught exception use `throw;` not `throw e;`. Using `throw e;` would throw a new copy of `e` (sliced to the static type `std::exception`) instead of rethrowing the original exception of type `std::runtime_error`. (But keep [Don't try to catch every exception in every function](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Re-not-always) and [Minimize the use of explicit `try`/`catch`](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Re-catch) in mind.)

##### Enforcement

Flag by-value exceptions if their types are part of a hierarchy (could require whole-program analysis to be perfect).

### <a name="Re-never-fail"></a>E.16: Destructors, deallocation, and `swap` must never fail

##### Reason

We don't know how to write reliable programs if a destructor, a swap, or a memory deallocation fails; that is, if it exits by an exception or simply doesn't perform its required action.

##### Example, don't

    class Connection {
        // ...
    public:
        ~Connection()   // Don't: very bad destructor
        {
            if (cannot_disconnect()) throw I_give_up{information};
            // ...
        }
    };

##### Note

Many have tried to write reliable code violating this rule for examples, such as a network connection that "refuses to close".
To the best of our knowledge nobody has found a general way of doing this.
Occasionally, for very specific examples, you can get away with setting some state for future cleanup.
For example, we might put a socket that does not want to close on a "bad socket" list,
to be examined by a regular sweep of the system state.
Every example we have seen of this is error-prone, specialized, and often buggy.

##### Note

The standard library assumes that destructors, deallocation functions (e.g., `operator delete`), and `swap` do not throw. If they do, basic standard-library invariants are broken.

##### Note

Deallocation functions, including `operator delete`, must be `noexcept`. `swap` functions must be `noexcept`.
Most destructors are implicitly `noexcept` by default.
Also, [make move operations `noexcept`](#Rc-move-noexcept).

##### Enforcement

Catch destructors, deallocation operations, and `swap`s that `throw`.
Catch such operations that are not `noexcept`.

**See also**: [discussion](#Sd-never-fail)

### <a name="Re-not-always"></a>E.17: Don't try to catch every exception in every function

##### Reason

Catching an exception in a function that cannot take a meaningful recovery action leads to complexity and waste.
Let an exception propagate until it reaches a function that can handle it.
Let cleanup actions on the unwinding path be handled by [RAII](#Re-raii).

##### Example, don't

    void f()   // bad
    {
        try {
            // ...
        }
        catch (...) {
            // no action
            throw;   // propagate exception
        }
    }

##### Enforcement

* Flag nested try-blocks.
* Flag source code files with a too high ratio of try-blocks to functions. (??? Problem: define "too high")

### <a name="Re-catch"></a>E.18: Minimize the use of explicit `try`/`catch`

##### Reason

 `try`/`catch` is verbose and non-trivial uses error-prone.
 `try`/`catch` can be a sign of unsystematic and/or low-level resource management or error handling.

##### Example, Bad

    void f(zstring s)
    {
        Gadget* p;
        try {
            p = new Gadget(s);
            // ...
            delete p;
        }
        catch (Gadget_construction_failure) {
            delete p;
            throw;
        }
    }

This code is messy.
There could be a leak from the naked pointer in the `try` block.
Not all exceptions are handled.
`deleting` an object that failed to construct is almost certainly a mistake.
Better:

    void f2(zstring s)
    {
        Gadget g {s};
    }

##### Alternatives

* proper resource handles and [RAII](#Re-raii)
* [`finally`](#Re-finally)

##### Enforcement

??? hard, needs a heuristic

### <a name="Re-finally"></a>E.19: Use a `final_action` object to express cleanup if no suitable resource handle is available

##### Reason

`finally` is less verbose and harder to get wrong than `try`/`catch`.

##### Example

    void f(int n)
    {
        void* p = malloc(n);
        auto _ = finally([p] { free(p); });
        // ...
    }

##### Note

`finally` is not as messy as `try`/`catch`, but it is still ad-hoc.
Prefer [proper resource management objects](#Re-raii).
Consider `finally` a last resort.

##### Note

Use of `finally` is a systematic and reasonably clean alternative to the old [`goto exit;` technique](#Re-no-throw-codes)
for dealing with cleanup where resource management is not systematic.

##### Enforcement

Heuristic: Detect `goto exit;`

### <a name="Re-no-throw-raii"></a>E.25: If you can't throw exceptions, simulate RAII for resource management

##### Reason

Even without exceptions, [RAII](#Re-raii) is usually the best and most systematic way of dealing with resources.

##### Note

Error handling using exceptions is the only complete and systematic way of handling non-local errors in C++.
In particular, non-intrusively signaling failure to construct an object requires an exception.
Signaling errors in a way that cannot be ignored requires exceptions.
If you can't use exceptions, simulate their use as best you can.

A lot of fear of exceptions is misguided.
When used for exceptional circumstances in code that is not littered with pointers and complicated control structures,
exception handling is almost always affordable (in time and space) and almost always leads to better code.
This, of course, assumes a good implementation of the exception handling mechanisms, which is not available on all systems.
There are also cases where the problems above do not apply, but exceptions cannot be used for other reasons.
Some hard-real-time systems are an example: An operation has to be completed within a fixed time with an error or a correct answer.
In the absence of appropriate time estimation tools, this is hard to guarantee for exceptions.
Such systems (e.g. flight control software) typically also ban the use of dynamic (heap) memory.

So, the primary guideline for error handling is "use exceptions and [RAII](#Re-raii)."
This section deals with the cases where you either do not have an efficient implementation of exceptions,
or have such a rat's nest of old-style code
(e.g., lots of pointers, ill-defined ownership, and lots of unsystematic error handling based on tests of error codes)
that it is infeasible to introduce simple and systematic exception handling.

Before condemning exceptions or complaining too much about their cost, consider examples of the use of [error codes](#Re-no-throw-codes).
Consider the cost and complexity of the use of error codes.
If performance is your worry, measure.

##### Example

Assume you wanted to write

    void func(zstring arg)
    {
        Gadget g {arg};
        // ...
    }

If the `gadget` isn't correctly constructed, `func` exits with an exception.
If we cannot throw an exception, we can simulate this RAII style of resource handling by adding a `valid()` member function to `Gadget`:

    error_indicator func(zstring arg)
    {
        Gadget g {arg};
        if (!g.valid()) return gadget_construction_error;
        // ...
        return 0;   // zero indicates "good"
    }

The problem is of course that the caller now has to remember to test the return value.

**See also**: [Discussion](#Sd-???)

##### Enforcement

Possible (only) for specific versions of this idea: e.g., test for systematic test of `valid()` after resource handle construction

### <a name="Re-no-throw-crash"></a>E.26: If you can't throw exceptions, consider failing fast

##### Reason

If you can't do a good job at recovering, at least you can get out before too much consequential damage is done.

**See also**: [Simulating RAII](#Re-no-throw-raii)

##### Note

If you cannot be systematic about error handling, consider "crashing" as a response to any error that cannot be handled locally.
That is, if you cannot recover from an error in the context of the function that detected it, call `abort()`, `quick_exit()`,
or a similar function that will trigger some sort of system restart.

In systems where you have lots of processes and/or lots of computers, you need to expect and handle fatal crashes anyway,
say from hardware failures.
In such cases, "crashing" is simply leaving error handling to the next level of the system.

##### Example

    void f(int n)
    {
        // ...
        p = static_cast<X*>(malloc(n * sizeof(X)));
        if (!p) abort();     // abort if memory is exhausted
        // ...
    }

Most programs cannot handle memory exhaustion gracefully anyway. This is roughly equivalent to

    void f(int n)
    {
        // ...
        p = new X[n];    // throw if memory is exhausted (by default, terminate)
        // ...
    }

Typically, it is a good idea to log the reason for the "crash" before exiting.

##### Enforcement

Awkward

### <a name="Re-no-throw-codes"></a>E.27: If you can't throw exceptions, use error codes systematically

##### Reason

Systematic use of any error-handling strategy minimizes the chance of forgetting to handle an error.

**See also**: [Simulating RAII](#Re-no-throw-raii)

##### Note

There are several issues to be addressed:

* how do you transmit an error indicator from out of a function?
* how do you release all resources from a function before doing an error exit?
* What do you use as an error indicator?

In general, returning an error indicator implies returning two values: The result and an error indicator.
The error indicator can be part of the object, e.g. an object can have a `valid()` indicator
or a pair of values can be returned.

##### Example

    Gadget make_gadget(int n)
    {
        // ...
    }

    void user()
    {
        Gadget g = make_gadget(17);
        if (!g.valid()) {
                // error handling
        }
        // ...
    }

This approach fits with [simulated RAII resource management](#Re-no-throw-raii).
The `valid()` function could return an `error_indicator` (e.g. a member of an `error_indicator` enumeration).

##### Example

What if we cannot or do not want to modify the `Gadget` type?
In that case, we must return a pair of values.
For example:

    std::pair<Gadget, error_indicator> make_gadget(int n)
    {
        // ...
    }

    void user()
    {
        auto r = make_gadget(17);
        if (!r.second) {
                // error handling
        }
        Gadget& g = r.first;
        // ...
    }

As shown, `std::pair` is a possible return type.
Some people prefer a specific type.
For example:

    Gval make_gadget(int n)
    {
        // ...
    }

    void user()
    {
        auto r = make_gadget(17);
        if (!r.err) {
                // error handling
        }
        Gadget& g = r.val;
        // ...
    }

One reason to prefer a specific return type is to have names for its members, rather than the somewhat cryptic `first` and `second`
and to avoid confusion with other uses of `std::pair`.

##### Example

In general, you must clean up before an error exit.
This can be messy:

    std::pair<int, error_indicator> user()
    {
        Gadget g1 = make_gadget(17);
        if (!g1.valid()) {
                return {0, g1_error};
        }

        Gadget g2 = make_gadget(17);
        if (!g2.valid()) {
                cleanup(g1);
                return {0, g2_error};
        }

        // ...

        if (all_foobar(g1, g2)) {
            cleanup(g1);
            cleanup(g2);
            return {0, foobar_error};
        // ...

        cleanup(g1);
        cleanup(g2);
        return {res, 0};
    }

Simulating RAII can be non-trivial, especially in functions with multiple resources and multiple possible errors.
A not uncommon technique is to gather cleanup at the end of the function to avoid repetition (note the extra scope around `g2` is undesirable but necessary to make the `goto` version compile):

    std::pair<int, error_indicator> user()
    {
        error_indicator err = 0;

        Gadget g1 = make_gadget(17);
        if (!g1.valid()) {
                err = g1_error;
                goto exit;
        }

        {
        Gadget g2 = make_gadget(17);
        if (!g2.valid()) {
                err = g2_error;
                goto exit;
        }

        if (all_foobar(g1, g2)) {
            err = foobar_error;
            goto exit;
        }
        // ...
        }

    exit:
      if (g1.valid()) cleanup(g1);
      if (g2.valid()) cleanup(g2);
      return {res, err};
    }

The larger the function, the more tempting this technique becomes.
`finally` can [ease the pain a bit](#Re-finally).
Also, the larger the program becomes the harder it is to apply an error-indicator-based error-handling strategy systematically.

We [prefer exception-based error handling](#Re-throw) and recommend [keeping functions short](#Rf-single).

**See also**: [Discussion](#Sd-???)

**See also**: [Returning multiple values](#Rf-out-multi)

##### Enforcement

Awkward.

### <a name="Re-no-throw"></a>E.28: Avoid error handling based on global state (e.g. `errno`)

##### Reason

Global state is hard to manage and it is easy to forget to check it.
When did you last test the return value of `printf()`?

**See also**: [Simulating RAII](#Re-no-throw-raii)

##### Example, bad

    int last_err;

    void f(int n)
    {
        // ...
        p = static_cast<X*>(malloc(n * sizeof(X)));
        if (!p) last_err = -1;     // error if memory is exhausted
        // ...
    }

##### Note

C-style error handling is based on the global variable `errno`, so it is essentially impossible to avoid this style completely.

##### Enforcement

Awkward.


### <a name="Re-specifications"></a>E.30: Don't use exception specifications

##### Reason

Exception specifications make error handling brittle, impose a run-time cost, and have been removed from the C++ standard.

##### Example

    int use(int arg)
        throw(X, Y)
    {
        // ...
        auto x = f(arg);
        // ...
    }

If `f()` throws an exception different from `X` and `Y` the unexpected handler is invoked, which by default terminates.
That's OK, but say that we have checked that this cannot happen and `f` is changed to throw a new exception `Z`,
we now have a crash on our hands unless we change `use()` (and re-test everything).
The snag is that `f()` may be in a library we do not control and the new exception is not anything that `use()` can do
anything about or is in any way interested in.
We can change `use()` to pass `Z` through, but now `use()`'s callers probably needs to be modified.
This quickly becomes unmanageable.
Alternatively, we can add a `try`-`catch` to `use()` to map `Z` into an acceptable exception.
This too, quickly becomes unmanageable.
Note that changes to the set of exceptions often happens at the lowest level of a system
(e.g., because of changes to a network library or some middleware), so changes "bubble up" through long call chains.
In a large code base, this could mean that nobody could update to a new version of a library until the last user was modified.
If `use()` is part of a library, it may not be possible to update it because a change could affect unknown clients.

The policy of letting exceptions propagate until they reach a function that potentially can handle it has proven itself over the years.

##### Note

No. This would not be any better had exception specifications been statically enforced.
For example, see [Stroustrup94](#Stroustrup94).

##### Note

If no exception may be thrown, use [`noexcept`](#Re-noexcept) or its equivalent `throw()`.

##### Enforcement

Flag every exception specification.

### <a name="Re_catch"></a>E.31: Properly order your `catch`-clauses

##### Reason

`catch`-clauses are evaluated in the order they appear and one clause can hide another.

##### Example

    void f()
    {
        // ...
        try {
                // ...
        }
        catch (Base& b) { /* ... */ }
        catch (Derived& d) { /* ... */ }
        catch (...) { /* ... */ }
        catch (std::exception& e){ /* ... */ }
    }

If `Derived`is derived from `Base` the `Derived`-handler will never be invoked.
The "catch everything" handler ensured that the `std::exception`-handler will never be invoked.

##### Enforcement

Flag all "hiding handlers".

# <a name="S-const"></a>Con: Constants and immutability

You can't have a race condition on a constant.
It is easier to reason about a program when many of the objects cannot change their values.
Interfaces that promises "no change" of objects passed as arguments greatly increase readability.

Constant rule summary:

* [Con.1: By default, make objects immutable](#Rconst-immutable)
* [Con.2: By default, make member functions `const`](#Rconst-fct)
* [Con.3: By default, pass pointers and references to `const`s](#Rconst-ref)
* [Con.4: Use `const` to define objects with values that do not change after construction](#Rconst-const)
* [Con.5: Use `constexpr` for values that can be computed at compile time](#Rconst-constexpr)

### <a name="Rconst-immutable"></a>Con.1: By default, make objects immutable

##### Reason

Immutable objects are easier to reason about, so make objects non-`const` only when there is a need to change their value.
Prevents accidental or hard-to-notice change of value.

##### Example

    for (const int i : c) cout << i << '\n';    // just reading: const

    for (int i : c) cout << i << '\n';          // BAD: just reading

##### Exception

Function arguments are rarely mutated, but also rarely declared const.
To avoid confusion and lots of false positives, don't enforce this rule for function arguments.

    void f(const char* const p); // pedantic
    void g(const int i);        // pedantic

Note that function parameter is a local variable so changes to it are local.

##### Enforcement

* Flag non-`const` variables that are not modified (except for parameters to avoid many false positives)

### <a name="Rconst-fct"></a>Con.2: By default, make member functions `const`

##### Reason

A member function should be marked `const` unless it changes the object's observable state.
This gives a more precise statement of design intent, better readability, more errors caught by the compiler, and sometimes more optimization opportunities.

##### Example, bad

    class Point {
        int x, y;
    public:
        int getx() { return x; }    // BAD, should be const as it doesn't modify the object's state
        // ...
    };

    void f(const Point& pt) {
        int x = pt.getx();          // ERROR, doesn't compile because getx was not marked const
    }

##### Note

It is not inherently bad to pass a pointer or reference to non-`const`,
but that should be done only when the called function is supposed to modify the object.
A reader of code must assume that a function that takes a "plain" `T*` or `T&` will modify the object referred to.
If it doesn't now, it might do so later without forcing recompilation.

##### Note

There are code/libraries that offer functions that declare a`T*` even though
those function do not modify that `T`.
This is a problem for people modernizing code.
You can

* update the library to be `const`-correct; preferred long-term solution
* "cast away `const`"; [best avoided](#Res-casts-const)
* provide a wrapper function

Example:

    void f(int* p);   // old code: f() does not modify `*p`
    void f(const int* p) { f(const_cast<int*>(p)); } // wrapper

Note that this wrapper solution is a patch that should be used only when the declaration of `f()` cannot be modified,
e.g. because it is in a library that you cannot modify.

##### Note

A `const` member function can modify the value of an object that is `mutable` or accessed through a pointer member.
A common use is to maintain a cache rather than repeatedly do a complicated computation.
For example, here is a `Date` that caches (memoizes) its string representation to simplify repeated uses:

    class Date {
    public:
        // ...
        const string& string_ref() const
        {
            if (string_val == "") compute_string_rep();
            return string_val;
        }
        // ...
    private:
        void compute_string_rep() const;    // compute string representation and place it in string_val
        mutable string string_val;
        // ...
    };

Another way of saying this is that `const`ness is not transitive.
It is possible for a `const` member function to change the value of `mutable` members and the value of objects accessed
through non-`const` pointers.
It is the job of the class to ensure such mutation is done only when it makes sense according to the semantics (invariants)
it offers to its users.

**See also**: [Pimpl](#Ri-pimpl)

##### Enforcement

* Flag a member function that is not marked `const`, but that does not perform a non-`const` operation on any member variable.

### <a name="Rconst-ref"></a>Con.3: By default, pass pointers and references to `const`s

##### Reason

 To avoid a called function unexpectedly changing the value.
 It's far easier to reason about programs when called functions don't modify state.

##### Example

    void f(char* p);        // does f modify *p? (assume it does)
    void g(const char* p);  // g does not modify *p

##### Note

It is not inherently bad to pass a pointer or reference to non-`const`,
but that should be done only when the called function is supposed to modify the object.

##### Note

[Do not cast away `const`](#Res-casts-const).

##### Enforcement

* Flag function that does not modify an object passed by  pointer or reference to non-`const`
* Flag a function that (using a cast) modifies an object passed by pointer or reference to `const`

### <a name="Rconst-const"></a>Con.4: Use `const` to define objects with values that do not change after construction

##### Reason

 Prevent surprises from unexpectedly changed object values.

##### Example

    void f()
    {
        int x = 7;
        const int y = 9;

        for (;;) {
            // ...
        }
        // ...
    }

As `x` is not `const`, we must assume that it is modified somewhere in the loop.

##### Enforcement

* Flag unmodified non-`const` variables.

### <a name="Rconst-constexpr"></a>Con.5: Use `constexpr` for values that can be computed at compile time

##### Reason

Better performance, better compile-time checking, guaranteed compile-time evaluation, no possibility of race conditions.

##### Example

    double x = f(2);            // possible run-time evaluation
    const double y = f(2);      // possible run-time evaluation
    constexpr double z = f(2);  // error unless f(2) can be evaluated at compile time

##### Note

See F.4.

##### Enforcement

* Flag `const` definitions with constant expression initializers.

# <a name="S-templates"></a>T: Templates and generic programming

Generic programming is programming using types and algorithms parameterized by types, values, and algorithms.
In C++, generic programming is supported by the `template` language mechanisms.

Arguments to generic functions are characterized by sets of requirements on the argument types and values involved.
In C++, these requirements are expressed by compile-time predicates called concepts.

Templates can also be used for meta-programming; that is, programs that compose code at compile time.

A central notion in generic programming is "concepts"; that is, requirements on template arguments presented as compile-time predicates.
"Concepts" are defined in an ISO Technical specification: [concepts](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
A draft of a set of standard-library concepts can be found in another ISO TS: [ranges](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf)
Concepts are supported in GCC 6.1 and later.
Consequently, we comment out uses of concepts in examples; that is, we use them as formalized comments only.
If you use GCC 6.1 or later, you can uncomment them.

Template use rule summary:

* [T.1: Use templates to raise the level of abstraction of code](#Rt-raise)
* [T.2: Use templates to express algorithms that apply to many argument types](#Rt-algo)
* [T.3: Use templates to express containers and ranges](#Rt-cont)
* [T.4: Use templates to express syntax tree manipulation](#Rt-expr)
* [T.5: Combine generic and OO techniques to amplify their strengths, not their costs](#Rt-generic-oo)

Concept use rule summary:

* [T.10: Specify concepts for all template arguments](#Rt-concepts)
* [T.11: Whenever possible use standard concepts](#Rt-std-concepts)
* [T.12: Prefer concept names over `auto` for local variables](#Rt-auto)
* [T.13: Prefer the shorthand notation for simple, single-type argument concepts](#Rt-shorthand)
* ???

Concept definition rule summary:

* [T.20: Avoid "concepts" without meaningful semantics](#Rt-low)
* [T.21: Require a complete set of operations for a concept](#Rt-complete)
* [T.22: Specify axioms for concepts](#Rt-axiom)
* [T.23: Differentiate a refined concept from its more general case by adding new use patterns](#Rt-refine)
* [T.24: Use tag classes or traits to differentiate concepts that differ only in semantics](#Rt-tag)
* [T.25: Avoid complementary constraints](#Rt-not)
* [T.26: Prefer to define concepts in terms of use-patterns rather than simple syntax](#Rt-use)
* [T.30: Use concept negation (`!C<T>`) sparingly to express a minor difference](#Rt-not)
* [T.31: Use concept disjunction (`C1<T> || C2<T>`) sparingly to express alternatives](#Rt-or)
* ???

Template interface rule summary:

* [T.40: Use function objects to pass operations to algorithms](#Rt-fo)
* [T.41: Require only essential properties in a template's concepts](#Rt-essential)
* [T.42: Use template aliases to simplify notation and hide implementation details](#Rt-alias)
* [T.43: Prefer `using` over `typedef` for defining aliases](#Rt-using)
* [T.44: Use function templates to deduce class template argument types (where feasible)](#Rt-deduce)
* [T.46: Require template arguments to be at least `Regular` or `SemiRegular`](#Rt-regular)
* [T.47: Avoid highly visible unconstrained templates with common names](#Rt-visible)
* [T.48: If your compiler does not support concepts, fake them with `enable_if`](#Rt-concept-def)
* [T.49: Where possible, avoid type-erasure](#Rt-erasure)

Template definition rule summary:

* [T.60: Minimize a template's context dependencies](#Rt-depend)
* [T.61: Do not over-parameterize members (SCARY)](#Rt-scary)
* [T.62: Place non-dependent class template members in a non-templated base class](#Rt-nondependent)
* [T.64: Use specialization to provide alternative implementations of class templates](#Rt-specialization)
* [T.65: Use tag dispatch to provide alternative implementations of functions](#Rt-tag-dispatch)
* [T.67: Use specialization to provide alternative implementations for irregular types](#Rt-specialization2)
* [T.68: Use `{}` rather than `()` within templates to avoid ambiguities](#Rt-cast)
* [T.69: Inside a template, don't make an unqualified nonmember function call unless you intend it to be a customization point](#Rt-customization)

Template and hierarchy rule summary:

* [T.80: Do not naively templatize a class hierarchy](#Rt-hier)
* [T.81: Do not mix hierarchies and arrays](#Rt-array) // ??? somewhere in "hierarchies"
* [T.82: Linearize a hierarchy when virtual functions are undesirable](#Rt-linear)
* [T.83: Do not declare a member function template virtual](#Rt-virtual)
* [T.84: Use a non-template core implementation to provide an ABI-stable interface](#Rt-abi)
* [T.??: ????](#Rt-???)

Variadic template rule summary:

* [T.100: Use variadic templates when you need a function that takes a variable number of arguments of a variety of types](#Rt-variadic)
* [T.101: ??? How to pass arguments to a variadic template ???](#Rt-variadic-pass)
* [T.102: ??? How to process arguments to a variadic template ???](#Rt-variadic-process)
* [T.103: Don't use variadic templates for homogeneous argument lists](#Rt-variadic-not)
* [T.??: ????](#Rt-???)

Metaprogramming rule summary:

* [T.120: Use template metaprogramming only when you really need to](#Rt-metameta)
* [T.121: Use template metaprogramming primarily to emulate concepts](#Rt-emulate)
* [T.122: Use templates (usually template aliases) to compute types at compile time](#Rt-tmp)
* [T.123: Use `constexpr` functions to compute values at compile time](#Rt-fct)
* [T.124: Prefer to use standard-library TMP facilities](#Rt-std-tmp)
* [T.125: If you need to go beyond the standard-library TMP facilities, use an existing library](#Rt-lib)
* [T.??: ????](#Rt-???)

Other template rules summary:

* [T.140: Name all operations with potential for reuse](#Rt-name)
* [T.141: Use an unnamed lambda if you need a simple function object in one place only](#Rt-lambda)
* [T.142: Use template variables to simplify notation](#Rt-var)
* [T.143: Don't write unintentionally nongeneric code](#Rt-nongeneric)
* [T.144: Don't specialize function templates](#Rt-specialize-function)
* [T.150: Check that a class matches a concept using `static_assert`](#Rt-check-class)
* [T.??: ????](#Rt-???)

## <a name="SS-GP"></a>T.gp: Generic programming

Generic programming is programming using types and algorithms parameterized by types, values, and algorithms.

### <a name="Rt-raise"></a>T.1: Use templates to raise the level of abstraction of code

##### Reason

Generality. Reuse. Efficiency. Encourages consistent definition of user types.

##### Example, bad

Conceptually, the following requirements are wrong because what we want of `T` is more than just the very low-level concepts of "can be incremented" or "can be added":

    template<typename T>
        // requires Incrementable<T>
    T sum1(vector<T>& v, T s)
    {
        for (auto x : v) s += x;
        return s;
    }

    template<typename T>
        // requires Simple_number<T>
    T sum2(vector<T>& v, T s)
    {
        for (auto x : v) s = s + x;
        return s;
    }

Assuming that `Incrementable` does not support `+` and `Simple_number` does not support `+=`, we have overconstrained implementers of `sum1` and `sum2`.
And, in this case, missed an opportunity for a generalization.

##### Example

    template<typename T>
        // requires Arithmetic<T>
    T sum(vector<T>& v, T s)
    {
        for (auto x : v) s += x;
        return s;
    }

Assuming that `Arithmetic` requires both `+` and `+=`, we have constrained the user of `sum` to provide a complete arithmetic type.
That is not a minimal requirement, but it gives the implementer of algorithms much needed freedom and ensures that any `Arithmetic` type
can be used for a wide variety of algorithms.

For additional generality and reusability, we could also use a more general `Container` or `Range` concept instead of committing to only one container, `vector`.

##### Note

If we define a template to require exactly the operations required for a single implementation of a single algorithm
(e.g., requiring just `+=` rather than also `=` and `+`) and only those, we have overconstrained maintainers.
We aim to minimize requirements on template arguments, but the absolutely minimal requirements of an implementation is rarely a meaningful concept.

##### Note

Templates can be used to express essentially everything (they are Turing complete), but the aim of generic programming (as expressed using templates)
is to efficiently generalize operations/algorithms over a set of types with similar semantic properties.

##### Note

The `requires` in the comments are uses of `concepts`.
"Concepts" are defined in an ISO Technical specification: [concepts](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
Concepts are supported in GCC 6.1 and later.
Consequently, we comment out uses of concepts in examples; that is, we use them as formalized comments only.
If you use GCC 6.1 or later, you can uncomment them.

##### Enforcement

* Flag algorithms with "overly simple" requirements, such as direct use of specific operators without a concept.
* Do not flag the definition of the "overly simple" concepts themselves; they may simply be building blocks for more useful concepts.

### <a name="Rt-algo"></a>T.2: Use templates to express algorithms that apply to many argument types

##### Reason

Generality. Minimizing the amount of source code. Interoperability. Reuse.

##### Example

That's the foundation of the STL. A single `find` algorithm easily works with any kind of input range:

    template<typename Iter, typename Val>
        // requires Input_iterator<Iter>
        //       && Equality_comparable<Value_type<Iter>, Val>
    Iter find(Iter b, Iter e, Val v)
    {
        // ...
    }

##### Note

Don't use a template unless you have a realistic need for more than one template argument type.
Don't overabstract.

##### Enforcement

??? tough, probably needs a human

### <a name="Rt-cont"></a>T.3: Use templates to express containers and ranges

##### Reason

Containers need an element type, and expressing that as a template argument is general, reusable, and type safe.
It also avoids brittle or inefficient workarounds. Convention: That's the way the STL does it.

##### Example

    template<typename T>
        // requires Regular<T>
    class Vector {
        // ...
        T* elem;   // points to sz Ts
        int sz;
    };

    Vector<double> v(10);
    v[7] = 9.9;

##### Example, bad

    class Container {
        // ...
        void* elem;   // points to size elements of some type
        int sz;
    };

    Container c(10, sizeof(double));
    ((double*) c.elem)[7] = 9.9;

This doesn't directly express the intent of the programmer and hides the structure of the program from the type system and optimizer.

Hiding the `void*` behind macros simply obscures the problems and introduces new opportunities for confusion.

**Exceptions**: If you need an ABI-stable interface, you might have to provide a base implementation and express the (type-safe) template in terms of that.
See [Stable base](#Rt-abi).

##### Enforcement

* Flag uses of `void*`s and casts outside low-level implementation code

### <a name="Rt-expr"></a>T.4: Use templates to express syntax tree manipulation

##### Reason

 ???

##### Example

    ???

**Exceptions**: ???

### <a name="Rt-generic-oo"></a>T.5: Combine generic and OO techniques to amplify their strengths, not their costs

##### Reason

Generic and OO techniques are complementary.

##### Example

Static helps dynamic: Use static polymorphism to implement dynamically polymorphic interfaces.

    class Command {
        // pure virtual functions
    };

    // implementations
    template</*...*/>
    class ConcreteCommand : public Command {
        // implement virtuals
    };

##### Example

Dynamic helps static: Offer a generic, comfortable, statically bound interface, but internally dispatch dynamically, so you offer a uniform object layout.
Examples include type erasure as with `std::shared_ptr`'s deleter (but [don't overuse type erasure](#Rt-erasure)).

##### Note

In a class template, nonvirtual functions are only instantiated if they're used -- but virtual functions are instantiated every time.
This can bloat code size, and may overconstrain a generic type by instantiating functionality that is never needed.
Avoid this, even though the standard-library facets made this mistake.

##### See also

* ref ???
* ref ???
* ref ???

##### Enforcement

See the reference to more specific rules.

## <a name="SS-concepts"></a>T.concepts: Concept rules

Concepts is a facility for specifying requirements for template arguments.
It is an [ISO technical specification](#Ref-conceptsTS), but currently supported only by GCC.
Concepts are, however, crucial in the thinking about generic programming and the basis of much work on future C++ libraries
(standard and other).

This section assumes concept support

Concept use rule summary:

* [T.10: Specify concepts for all template arguments](#Rt-concepts)
* [T.11: Whenever possible use standard concepts](#Rt-std-concepts)
* [T.12: Prefer concept names over `auto`](#Rt-auto)
* [T.13: Prefer the shorthand notation for simple, single-type argument concepts](#Rt-shorthand)
* ???

Concept definition rule summary:

* [T.20: Avoid "concepts" without meaningful semantics](#Rt-low)
* [T.21: Require a complete set of operations for a concept](#Rt-complete)
* [T.22: Specify axioms for concepts](#Rt-axiom)
* [T.23: Differentiate a refined concept from its more general case by adding new use patterns](#Rt-refine)
* [T.24: Use tag classes or traits to differentiate concepts that differ only in semantics](#Rt-tag)
* [T.25: Avoid complimentary constraints](#Rt-not)
* [T.26: Prefer to define concepts in terms of use-patterns rather than simple syntax](#Rt-use)
* ???

## <a name="SS-concept-use"></a>T.con-use: Concept use

### <a name="Rt-concepts"></a>T.10: Specify concepts for all template arguments

##### Reason

Correctness and readability.
The assumed meaning (syntax and semantics) of a template argument is fundamental to the interface of a template.
A concept dramatically improves documentation and error handling for the template.
Specifying concepts for template arguments is a powerful design tool.

##### Example

    template<typename Iter, typename Val>
    //    requires Input_iterator<Iter>
    //             && Equality_comparable<Value_type<Iter>, Val>
    Iter find(Iter b, Iter e, Val v)
    {
        // ...
    }

or equivalently and more succinctly:

    template<Input_iterator Iter, typename Val>
    //    requires Equality_comparable<Value_type<Iter>, Val>
    Iter find(Iter b, Iter e, Val v)
    {
        // ...
    }

##### Note

"Concepts" are defined in an ISO Technical specification: [concepts](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
A draft of a set of standard-library concepts can be found in another ISO TS: [ranges](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf)
Concepts are supported in GCC 6.1 and later.
Consequently, we comment out uses of concepts in examples; that is, we use them as formalized comments only.
If you use GCC 6.1 or later, you can uncomment them:

    template<typename Iter, typename Val>
        requires Input_iterator<Iter>
               && Equality_comparable<Value_type<Iter>, Val>
    Iter find(Iter b, Iter e, Val v)
    {
        // ...
    }

##### Note

Plain `typename` (or `auto`) is the least constraining concept.
It should be used only rarely when nothing more than "it's a type" can be assumed.
This is typically only needed when (as part of template metaprogramming code) we manipulate pure expression trees, postponing type checking.

**References**: TC++PL4, Palo Alto TR, Sutton

##### Enforcement

Flag template type arguments without concepts

### <a name="Rt-std-concepts"></a>T.11: Whenever possible use standard concepts

##### Reason

 "Standard" concepts (as provided by the [GSL](#S-GSL) and the [Ranges TS](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf), and hopefully soon the ISO standard itself)
save us the work of thinking up our own concepts, are better thought out than we can manage to do in a hurry, and improve interoperability.

##### Note

Unless you are creating a new generic library, most of the concepts you need will already be defined by the standard library.

##### Example (using TS concepts)

    template<typename T>
        // don't define this: Sortable is in the GSL
    concept Ordered_container = Sequence<T> && Random_access<Iterator<T>> && Ordered<Value_type<T>>;

    void sort(Ordered_container& s);

This `Ordered_container` is quite plausible, but it is very similar to the `Sortable` concept in the GSL (and the Range TS).
Is it better? Is it right? Does it accurately reflect the standard's requirements for `sort`?
It is better and simpler just to use `Sortable`:

    void sort(Sortable& s);   // better

##### Note

The set of "standard" concepts is evolving as we approach an ISO standard including concepts.

##### Note

Designing a useful concept is challenging.

##### Enforcement

Hard.

* Look for unconstrained arguments, templates that use "unusual"/non-standard concepts, templates that use "homebrew" concepts without axioms.
* Develop a concept-discovery tool (e.g., see [an early experiment](http://www.stroustrup.com/sle2010_webversion.pdf)).

### <a name="Rt-auto"></a>T.12: Prefer concept names over `auto` for local variables

##### Reason

 `auto` is the weakest concept. Concept names convey more meaning than just `auto`.

##### Example (using TS concepts)

    vector<string> v{ "abc", "xyz" };
    auto& x = v.front();     // bad
    String& s = v.front();   // good (String is a GSL concept)

##### Enforcement

* ???

### <a name="Rt-shorthand"></a>T.13: Prefer the shorthand notation for simple, single-type argument concepts

##### Reason

Readability. Direct expression of an idea.

##### Example (using TS concepts)

To say "`T` is `Sortable`":

    template<typename T>       // Correct but verbose: "The parameter is
    //    requires Sortable<T>   // of type T which is the name of a type
    void sort(T&);             // that is Sortable"

    template<Sortable T>       // Better (assuming support for concepts): "The parameter is of type T
    void sort(T&);             // which is Sortable"

    void sort(Sortable&);      // Best (assuming support for concepts): "The parameter is Sortable"

The shorter versions better match the way we speak. Note that many templates don't need to use the `template` keyword.

##### Note

"Concepts" are defined in an ISO Technical specification: [concepts](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
A draft of a set of standard-library concepts can be found in another ISO TS: [ranges](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf)
Concepts are supported in GCC 6.1 and later.
Consequently, we comment out uses of concepts in examples; that is, we use them as formalized comments only.
If you use a compiler that supports concepts (e.g., GCC 6.1 or later), you can remove the `//`.

##### Enforcement

* Not feasible in the short term when people convert from the `<typename T>` and `<class T`> notation.
* Later, flag declarations that first introduce a typename and then constrain it with a simple, single-type-argument concept.

## <a name="SS-concepts-def"></a>T.concepts.def: Concept definition rules

Defining good concepts is non-trivial.
Concepts are meant to represent fundamental concepts in an application domain (hence the name "concepts").
Similarly throwing together a set of syntactic constraints to be used for the arguments for a single class or algorithm is not what concepts were designed for
and will not give the full benefits of the mechanism.

Obviously, defining concepts will be most useful for code that can use an implementation (e.g., GCC 6.1 or later),
but defining concepts is in itself a useful design technique and help catch conceptual errors and clean up the concepts (sic!) of an implementation.

### <a name="Rt-low"></a>T.20: Avoid "concepts" without meaningful semantics

##### Reason

Concepts are meant to express semantic notions, such as "a number", "a range" of elements, and "totally ordered."
Simple constraints, such as "has a `+` operator" and "has a `>` operator" cannot be meaningfully specified in isolation
and should be used only as building blocks for meaningful concepts, rather than in user code.

##### Example, bad (using TS concepts)

    template<typename T>
    concept Addable = has_plus<T>;    // bad; insufficient

    template<Addable N> auto algo(const N& a, const N& b) // use two numbers
    {
        // ...
        return a + b;
    }

    int x = 7;
    int y = 9;
    auto z = algo(x, y);   // z = 16

    string xx = "7";
    string yy = "9";
    auto zz = algo(xx, yy);   // zz = "79"

Maybe the concatenation was expected. More likely, it was an accident. Defining minus equivalently would give dramatically different sets of accepted types.
This `Addable` violates the mathematical rule that addition is supposed to be commutative: `a+b == b+a`.

##### Note

The ability to specify a meaningful semantics is a defining characteristic of a true concept, as opposed to a syntactic constraint.

##### Example (using TS concepts)

    template<typename T>
    // The operators +, -, *, and / for a number are assumed to follow the usual mathematical rules
    concept Number = has_plus<T>
                     && has_minus<T>
                     && has_multiply<T>
                     && has_divide<T>;

    template<Number N> auto algo(const N& a, const N& b)
    {
        // ...
        return a + b;
    }

    int x = 7;
    int y = 9;
    auto z = algo(x, y);   // z = 16

    string xx = "7";
    string yy = "9";
    auto zz = algo(xx, yy);   // error: string is not a Number

##### Note

Concepts with multiple operations have far lower chance of accidentally matching a type than a single-operation concept.

##### Enforcement

* Flag single-operation `concepts` when used outside the definition of other `concepts`.
* Flag uses of `enable_if` that appears to simulate single-operation `concepts`.


### <a name="Rt-complete"></a>T.21: Require a complete set of operations for a concept

##### Reason

Ease of comprehension.
Improved interoperability.
Helps implementers and maintainers.

##### Note

This is a specific variant of the general rule that [a concept must make semantic sense](#Rt-low).

##### Example, bad (using TS concepts)

    template<typename T> concept Subtractable = requires(T a, T, b) { a-b; };

This makes no semantic sense.
You need at least `+` to make `-` meaningful and useful.

Examples of complete sets are

* `Arithmetic`: `+`, `-`, `*`, `/`, `+=`, `-=`, `*=`, `/=`
* `Comparable`: `<`, `>`, `<=`, `>=`, `==`, `!=`

##### Note

This rule applies whether we use direct language support for concepts or not.
It is a general design rule that even applies to non-templates:

    class Minimal {
        // ...
    };

    bool operator==(const Minimal&, const Minimal&);
    bool operator<(const Minimal&, const Minimal&);

    Minimal operator+(const Minimal&, const Minimal&);
    // no other operators

    void f(const Minimal& x, const Minimal& y)
    {
        if (!(x == y)) { /* ... */ }    // OK
        if (x != y) { /* ... */ }       // surprise! error

        while (!(x < y)) { /* ... */ }  // OK
        while (x >= y) { /* ... */ }    // surprise! error

        x = x + y;          // OK
        x += y;             // surprise! error
    }

This is minimal, but surprising and constraining for users.
It could even be less efficient.

The rule supports the view that a concept should reflect a (mathematically) coherent set of operations.

##### Example

    class Convenient {
        // ...
    };

    bool operator==(const Convenient&, const Convenient&);
    bool operator<(const Convenient&, const Convenient&);
    // ... and the other comparison operators ...

    Minimal operator+(const Convenient&, const Convenient&);
    // .. and the other arithmetic operators ...

    void f(const Convenient& x, const Convenient& y)
    {
        if (!(x == y)) { /* ... */ }    // OK
        if (x != y) { /* ... */ }       // OK

        while (!(x < y)) { /* ... */ }  // OK
        while (x >= y) { /* ... */ }    // OK

        x = x + y;     // OK
        x += y;        // OK
    }

It can be a nuisance to define all operators, but not hard.
Ideally, that rule should be language supported by giving you comparison operators by default.

##### Enforcement

* Flag classes that support "odd" subsets of a set of operators, e.g., `==` but not `!=` or `+` but not `-`.
  Yes, `std::string` is "odd", but it's too late to change that.


### <a name="Rt-axiom"></a>T.22: Specify axioms for concepts

##### Reason

A meaningful/useful concept has a semantic meaning.
Expressing these semantics in an informal, semi-formal, or formal way makes the concept comprehensible to readers and the effort to express it can catch conceptual errors.
Specifying semantics is a powerful design tool.

##### Example (using TS concepts)

    template<typename T>
        // The operators +, -, *, and / for a number are assumed to follow the usual mathematical rules
        // axiom(T a, T b) { a + b == b + a; a - a == 0; a * (b + c) == a * b + a * c; /*...*/ }
        concept Number = requires(T a, T b) {
            {a + b} -> T;   // the result of a + b is convertible to T
            {a - b} -> T;
            {a * b} -> T;
            {a / b} -> T;
        }

##### Note

This is an axiom in the mathematical sense: something that may be assumed without proof.
In general, axioms are not provable, and when they are the proof is often beyond the capability of a compiler.
An axiom may not be general, but the template writer may assume that it holds for all inputs actually used (similar to a precondition).

##### Note

In this context axioms are Boolean expressions.
See the [Palo Alto TR](#S-references) for examples.
Currently, C++ does not support axioms (even the ISO Concepts TS), so we have to make do with comments for a longish while.
Once language support is available, the `//` in front of the axiom can be removed

##### Note

The GSL concepts have well-defined semantics; see the Palo Alto TR and the Ranges TS.

##### Exception (using TS concepts)

Early versions of a new "concept" still under development will often just define simple sets of constraints without a well-specified semantics.
Finding good semantics can take effort and time.
An incomplete set of constraints can still be very useful:

    // balancer for a generic binary tree
    template<typename Node> concept bool Balancer = requires(Node* p) {
        add_fixup(p);
        touch(p);
        detach(p);
    }

So a `Balancer` must supply at least thee operations on a tree `Node`,
but we are not yet ready to specify detailed semantics because a new kind of balanced tree might require more operations
and the precise general semantics for all nodes is hard to pin down in the early stages of design.

A "concept" that is incomplete or without a well-specified semantics can still be useful.
For example, it allows for some checking during initial experimentation.
However, it should not be assumed to be stable.
Each new use case may require such an incomplete concept to be improved.

##### Enforcement

* Look for the word "axiom" in concept definition comments

### <a name="Rt-refine"></a>T.23: Differentiate a refined concept from its more general case by adding new use patterns.

##### Reason

Otherwise they cannot be distinguished automatically by the compiler.

##### Example (using TS concepts)

    template<typename I>
    concept bool Input_iter = requires(I iter) { ++iter; };

    template<typename I>
    concept bool Fwd_iter = Input_iter<I> && requires(I iter) { iter++; }

The compiler can determine refinement based on the sets of required operations (here, suffix `++`).
This decreases the burden on implementers of these types since
they do not need any special declarations to "hook into the concept".
If two concepts have exactly the same requirements, they are logically equivalent (there is no refinement).

##### Enforcement

* Flag a concept that has exactly the same requirements as another already-seen concept (neither is more refined).
To disambiguate them, see [T.24](#Rt-tag).

### <a name="Rt-tag"></a>T.24: Use tag classes or traits to differentiate concepts that differ only in semantics.

##### Reason

Two concepts requiring the same syntax but having different semantics leads to ambiguity unless the programmer differentiates them.

##### Example (using TS concepts)

    template<typename I>    // iterator providing random access
    concept bool RA_iter = ...;

    template<typename I>    // iterator providing random access to contiguous data
    concept bool Contiguous_iter =
        RA_iter<I> && is_contiguous<I>::value;  // using is_contiguous trait

The programmer (in a library) must define `is_contiguous` (a trait) appropriately.

Wrapping a tag class into a concept leads to a simpler expression of this idea:

    template<typename I> concept Contiguous = is_contiguous<I>::value;

    template<typename I>
    concept bool Contiguous_iter = RA_iter<I> && Contiguous<I>;

The programmer (in a library) must define `is_contiguous` (a trait) appropriately.

##### Note

Traits can be trait classes or type traits.
These can be user-defined or standard-library ones.
Prefer the standard-library ones.

##### Enforcement

* The compiler flags ambiguous use of identical concepts.
* Flag the definition of identical concepts.

### <a name="Rt-not"></a>T.25: Avoid complementary constraints

##### Reason

Clarity. Maintainability.
Functions with complementary requirements expressed using negation are brittle.

##### Example (using TS concepts)

Initially, people will try to define functions with complementary requirements:

    template<typename T>
        requires !C<T>    // bad
    void f();

    template<typename T>
        requires C<T>
    void f();

This is better:

    template<typename T>   // general template
        void f();

    template<typename T>   // specialization by concept
        requires C<T>
    void f();

The compiler will choose the unconstrained template only when `C<T>` is
unsatisfied. If you do not want to (or cannot) define an unconstrained
version of `f()`, then delete it.

    template<typename T>
    void f() = delete;

The compiler will select the overload and emit an appropriate error.

##### Note

Complementary constraints are unfortunately common in `enable_if` code:

    template<typename T>
    enable_if<!C<T>, void>   // bad
    f();

    template<typename T>
    enable_if<C<T>, void>
    f();


##### Note

Complementary requirements on one requirements is sometimes (wrongly) considered manageable.
However, for two or more requirements the number of definitions needs can go up exponentially (2,4,9,16,...):

    C1<T> && C2<T>
    !C1<T> && C2<T>
    C1<T> && !C2<T>
    !C1<T> && !C2<T>

Now the opportunities for errors multiply.

##### Enforcement

* Flag pairs of functions with `C<T>` and `!C<T>` constraints

### <a name="Rt-use"></a>T.26: Prefer to define concepts in terms of use-patterns rather than simple syntax

##### Reason

The definition is more readable and corresponds directly to what a user has to write.
Conversions are taken into account. You don't have to remember the names of all the type traits.

##### Example (using TS concepts)

You might be tempted to define a concept `Equality` like this:

    template<typename T> concept Equality = has_equal<T> && has_not_equal<T>;

Obviously, it would be better and easier just to use the standard `EqualityComparable`,
but - just as an example - if you had to define such a concept, prefer:

    template<typename T> concept Equality = requires(T a, T b) {
        bool == { a == b }
        bool == { a != b }
        // axiom { !(a == b) == (a != b) }
        // axiom { a = b; => a == b }  // => means "implies"
    }

as opposed to defining two meaningless concepts `has_equal` and `has_not_equal` just as helpers in the definition of `Equality`.
By "meaningless" we mean that we cannot specify the semantics of `has_equal` in isolation.

##### Enforcement

???

## <a name="SS-temp-interface"></a>Template interfaces

Over the years, programming with templates have suffered from a weak distinction between the interface of a template
and its implementation.
Before concepts, that distinction had no direct language support.
However, the interface to a template is a critical concept - a contract between a user and an implementer - and should be carefully designed.

### <a name="Rt-fo"></a>T.40: Use function objects to pass operations to algorithms

##### Reason

Function objects can carry more information through an interface than a "plain" pointer to function.
In general, passing function objects gives better performance than passing pointers to functions.

##### Example (using TS concepts)

    bool greater(double x, double y) { return x > y; }
    sort(v, greater);                                    // pointer to function: potentially slow
    sort(v, [](double x, double y) { return x > y; });   // function object
    sort(v, std::greater<>);                             // function object

    bool greater_than_7(double x) { return x > 7; }
    auto x = find_if(v, greater_than_7);                 // pointer to function: inflexible
    auto y = find_if(v, [](double x) { return x > 7; }); // function object: carries the needed data
    auto z = find_if(v, Greater_than<double>(7));        // function object: carries the needed data

You can, of course, generalize those functions using `auto` or (when and where available) concepts. For example:

    auto y1 = find_if(v, [](Ordered x) { return x > 7; }); // require an ordered type
    auto z1 = find_if(v, [](auto x) { return x > 7; });    // hope that the type has a >

##### Note

Lambdas generate function objects.

##### Note

The performance argument depends on compiler and optimizer technology.

##### Enforcement

* Flag pointer to function template arguments.
* Flag pointers to functions passed as arguments to a template (risk of false positives).


### <a name="Rt-essential"></a>T.41: Require only essential properties in a template's concepts

##### Reason

Keep interfaces simple and stable.

##### Example (using TS concepts)

Consider, a `sort` instrumented with (oversimplified) simple debug support:

    void sort(Sortable& s)  // sort sequence s
    {
        if (debug) cerr << "enter sort( " << s <<  ")\n";
        // ...
        if (debug) cerr << "exit sort( " << s <<  ")\n";
    }

Should this be rewritten to:

    template<Sortable S>
        requires Streamable<S>
    void sort(S& s)  // sort sequence s
    {
        if (debug) cerr << "enter sort( " << s <<  ")\n";
        // ...
        if (debug) cerr << "exit sort( " << s <<  ")\n";
    }

After all, there is nothing in `Sortable` that requires `iostream` support.
On the other hand, there is nothing in the fundamental idea of sorting that says anything about debugging.

##### Note

If we require every operation used to be listed among the requirements, the interface becomes unstable:
Every time we change the debug facilities, the usage data gathering, testing support, error reporting, etc.,
the definition of the template would need change and every use of the template would have to be recompiled.
This is cumbersome, and in some environments infeasible.

Conversely, if we use an operation in the implementation that is not guaranteed by concept checking,
we may get a late compile-time error.

By not using concept checking for properties of a template argument that is not considered essential,
we delay checking until instantiation time.
We consider this a worthwhile tradeoff.

Note that using non-local, non-dependent names (such as `debug` and `cerr`) also introduces context dependencies that may lead to "mysterious" errors.

##### Note

It can be hard to decide which properties of a type are essential and which are not.

##### Enforcement

???

### <a name="Rt-alias"></a>T.42: Use template aliases to simplify notation and hide implementation details

##### Reason

Improved readability.
Implementation hiding.
Note that template aliases replace many uses of traits to compute a type.
They can also be used to wrap a trait.

##### Example

    template<typename T, size_t N>
    class Matrix {
        // ...
        using Iterator = typename std::vector<T>::iterator;
        // ...
    };

This saves the user of `Matrix` from having to know that its elements are stored in a `vector` and also saves the user from repeatedly typing `typename std::vector<T>::`.

##### Example

    template<typename T>
    void user(T& c)
    {
        // ...
        typename container_traits<T>::value_type x; // bad, verbose
        // ...
    }

    template<typename T>
    using Value_type = typename container_traits<T>::value_type;


This saves the user of `Value_type` from having to know the technique used to implement `value_type`s.

    template<typename T>
    void user2(T& c)
    {
        // ...
        Value_type<T> x;
        // ...
    }

##### Note

A simple, common use could be expressed: "Wrap traits!"

##### Enforcement

* Flag use of `typename` as a disambiguator outside `using` declarations.
* ???

### <a name="Rt-using"></a>T.43: Prefer `using` over `typedef` for defining aliases

##### Reason

Improved readability: With `using`, the new name comes first rather than being embedded somewhere in a declaration.
Generality: `using` can be used for template aliases, whereas `typedef`s can't easily be templates.
Uniformity: `using` is syntactically similar to `auto`.

##### Example

    typedef int (*PFI)(int);   // OK, but convoluted

    using PFI2 = int (*)(int);   // OK, preferred

    template<typename T>
    typedef int (*PFT)(T);      // error

    template<typename T>
    using PFT2 = int (*)(T);   // OK

##### Enforcement

* Flag uses of `typedef`. This will give a lot of "hits" :-(

### <a name="Rt-deduce"></a>T.44: Use function templates to deduce class template argument types (where feasible)

##### Reason

Writing the template argument types explicitly can be tedious and unnecessarily verbose.

##### Example

    tuple<int, string, double> t1 = {1, "Hamlet", 3.14};   // explicit type
    auto t2 = make_tuple(1, "Ophelia"s, 3.14);         // better; deduced type

Note the use of the `s` suffix to ensure that the string is a `std::string`, rather than a C-style string.

##### Note

Since you can trivially write a `make_T` function, so could the compiler. Thus, `make_T` functions may become redundant in the future.

##### Exception

Sometimes there isn't a good way of getting the template arguments deduced and sometimes, you want to specify the arguments explicitly:

    vector<double> v = { 1, 2, 3, 7.9, 15.99 };
    list<Record*> lst;

##### Note

Note that C++17 will make this rule redundant by allowing the template arguments to be deduced directly from constructor arguments:
[Template parameter deduction for constructors (Rev. 3)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0091r1.html).
For example:

    tuple t1 = {1, "Hamlet"s, 3.14}; // deduced: tuple<int, string, double>

##### Enforcement

Flag uses where an explicitly specialized type exactly matches the types of the arguments used.

### <a name="Rt-regular"></a>T.46: Require template arguments to be at least `Regular` or `SemiRegular`

##### Reason

 Readability.
 Preventing surprises and errors.
 Most uses support that anyway.

##### Example

    class X {
            // ...
    public:
        explicit X(int);
        X(const X&);            // copy
        X operator=(const X&);
        X(X&&) noexcept;                 // move
        X& operator=(X&&) noexcept;
        ~X();
        // ... no more constructors ...
    };

    X x {1};    // fine
    X y = x;      // fine
    std::vector<X> v(10); // error: no default constructor

##### Note

Semiregular requires default constructible.

##### Enforcement

* Flag types that are not at least `SemiRegular`.

### <a name="Rt-visible"></a>T.47: Avoid highly visible unconstrained templates with common names

##### Reason

 An unconstrained template argument is a perfect match for anything so such a template can be preferred over more specific types that require minor conversions.
 This is particularly annoying/dangerous when ADL is used.
 Common names make this problem more likely.

##### Example

    namespace Bad {
        struct S { int m; };
        template<typename T1, typename T2>
        bool operator==(T1, T2) { cout << "Bad\n"; return true; }
    }

    namespace T0 {
        bool operator==(int, Bad::S) { cout << "T0\n"; return true; }  // compare to int

        void test()
        {
            Bad::S bad{ 1 };
            vector<int> v(10);
            bool b = 1 == bad;
            bool b2 = v.size() == bad;
        }
    }

This prints `T0` and `Bad`.

Now the `==` in `Bad` was designed to cause trouble, but would you have spotted the problem in real code?
The problem is that `v.size()` returns an `unsigned` integer so that a conversion is needed to call the local `==`;
the `==` in `Bad` requires no conversions.
Realistic types, such as the standard-library iterators can be made to exhibit similar anti-social tendencies.

##### Note

If an unconstrained template is defined in the same namespace as a type,
that unconstrained template can be found by ADL (as happened in the example).
That is, it is highly visible.

##### Note

This rule should not be necessary, but the committee cannot agree to exclude unconstrained templated from ADL.

Unfortunately this will get many false positives; the standard library violates this widely, by putting many unconstrained templates and types into the single namespace `std`.


##### Enforcement

Flag templates defined in a namespace where concrete types are also defined (maybe not feasible until we have concepts).


### <a name="Rt-concept-def"></a>T.48: If your compiler does not support concepts, fake them with `enable_if`

##### Reason

Because that's the best we can do without direct concept support.
`enable_if` can be used to conditionally define functions and to select among a set of functions.

##### Example

    enable_if<???>

##### Note

Beware of [complementary constraints](# T.25).
Faking concept overloading using `enable_if` sometimes forces us to use that error-prone design technique.

##### Enforcement

???

### <a name="Rt-erasure"></a>T.49: Where possible, avoid type-erasure

##### Reason

Type erasure incurs an extra level of indirection by hiding type information behind a separate compilation boundary.

##### Example

    ???

**Exceptions**: Type erasure is sometimes appropriate, such as for `std::function`.

##### Enforcement

???


##### Note


## <a name="SS-temp-def"></a>T.def: Template definitions

A template definition (class or function) can contain arbitrary code, so only a comprehensive review of C++ programming techniques would cover this topic.
However, this section focuses on what is specific to template implementation.
In particular, it focuses on a template definition's dependence on its context.

### <a name="Rt-depend"></a>T.60: Minimize a template's context dependencies

##### Reason

Eases understanding.
Minimizes errors from unexpected dependencies.
Eases tool creation.

##### Example

    template<typename C>
    void sort(C& c)
    {
        std::sort(begin(c), end(c)); // necessary and useful dependency
    }

    template<typename Iter>
    Iter algo(Iter first, Iter last) {
        for (; first != last; ++first) {
            auto x = sqrt(*first); // potentially surprising dependency: which sqrt()?
            helper(first, x);      // potentially surprising dependency:
                                   // helper is chosen based on first and x
            TT var = 7;            // potentially surprising dependency: which TT?
        }
    }

##### Note

Templates typically appear in header files so their context dependencies are more vulnerable to `#include` order dependencies than functions in `.cpp` files.

##### Note

Having a template operate only on its arguments would be one way of reducing the number of dependencies to a minimum, but that would generally be unmanageable.
For example, an algorithm usually uses other algorithms and invoke operations that does not exclusively operate on arguments.
And don't get us started on macros!

**See also**: [T.69](#Rt-customization)

##### Enforcement

??? Tricky

### <a name="Rt-scary"></a>T.61: Do not over-parameterize members (SCARY)

##### Reason

A member that does not depend on a template parameter cannot be used except for a specific template argument.
This limits use and typically increases code size.

##### Example, bad

    template<typename T, typename A = std::allocator{}>
        // requires Regular<T> && Allocator<A>
    class List {
    public:
        struct Link {   // does not depend on A
            T elem;
            T* pre;
            T* suc;
        };

        using iterator = Link*;

        iterator first() const { return head; }

        // ...
    private:
        Link* head;
    };

    List<int> lst1;
    List<int, My_allocator> lst2;

This looks innocent enough, but now `Link` formally depends on the allocator (even though it doesn't use the allocator). This forces redundant instantiations that can be surprisingly costly in some real-world scenarios.
Typically, the solution is to make what would have been a nested class non-local, with its own minimal set of template parameters.

    template<typename T>
    struct Link {
        T elem;
        T* pre;
        T* suc;
    };

    template<typename T, typename A = std::allocator{}>
        // requires Regular<T> && Allocator<A>
    class List2 {
    public:
        using iterator = Link<T>*;

        iterator first() const { return head; }

        // ...
    private:
        Link* head;
    };

    List<int> lst1;
    List<int, My_allocator> lst2;

Some people found the idea that the `Link` no longer was hidden inside the list scary, so we named the technique
[SCARY](http://www.open-std.org/jtc1/sc22/WG21/docs/papers/2009/n2911.pdf).From that academic paper:
"The acronym SCARY describes assignments and initializations that are Seemingly erroneous (appearing Constrained by conflicting generic parameters), but Actually work with the Right implementation (unconstrained bY the conflict due to minimized dependencies."

##### Enforcement

* Flag member types that do not depend on every template argument
* Flag member functions that do not depend on every template argument

### <a name="Rt-nondependent"></a>T.62: Place non-dependent class template members in a non-templated base class

##### Reason

 Allow the base class members to be used without specifying template arguments and without template instantiation.

##### Example

    template<typename T>
    class Foo {
    public:
        enum { v1, v2 };
        // ...
    };

???

    struct Foo_base {
        enum { v1, v2 };
        // ...
    };

    template<typename T>
    class Foo : public Foo_base {
    public:
        // ...
    };

##### Note

A more general version of this rule would be
"If a template class member depends on only N template parameters out of M, place it in a base class with only N parameters."
For N == 1, we have a choice of a base class of a class in the surrounding scope as in [T.61](#Rt-scary).

??? What about constants? class statics?

##### Enforcement

* Flag ???

### <a name="Rt-specialization"></a>T.64: Use specialization to provide alternative implementations of class templates

##### Reason

A template defines a general interface.
Specialization offers a powerful mechanism for providing alternative implementations of that interface.

##### Example

    ??? string specialization (==)

    ??? representation specialization ?

##### Note

???

##### Enforcement

???

### <a name="Rt-tag-dispatch"></a>T.65: Use tag dispatch to provide alternative implementations of a function

##### Reason

* A template defines a general interface.
* Tag dispatch allows us to select implementations based on specific properties of an argument type.
* Performance.

##### Example

This is a simplified version of `std::copy` (ignoring the possibility of non-contiguous sequences)

    struct pod_tag {};
    struct non_pod_tag {};

    template<class T> struct copy_trait { using tag = non_pod_tag; };   // T is not "plain old data"

    template<> struct copy_trait<int> { using tag = pod_tag; };         // int is "plain old data"

    template<class Iter>
    Out copy_helper(Iter first, Iter last, Iter out, pod_tag)
    {
        // use memmove
    }

    template<class Iter>
    Out copy_helper(Iter first, Iter last, Iter out, non_pod_tag)
    {
        // use loop calling copy constructors
    }

    template<class Itert>
    Out copy(Iter first, Iter last, Iter out)
    {
        return copy_helper(first, last, out, typename copy_trait<Iter>::tag{})
    }

    void use(vector<int>& vi, vector<int>& vi2, vector<string>& vs, vector<string>& vs2)
    {
        copy(vi.begin(), vi.end(), vi2.begin()); // uses memmove
        copy(vs.begin(), vs.end(), vs2.begin()); // uses a loop calling copy constructors
    }

This is a general and powerful technique for compile-time algorithm selection.

##### Note

When `concept`s become widely available such alternatives can be distinguished directly:

    template<class Iter>
        requires Pod<Value_type<iter>>
    Out copy_helper(In, first, In last, Out out)
    {
        // use memmove
    }

    template<class Iter>
    Out copy_helper(In, first, In last, Out out)
    {
        // use loop calling copy constructors
    }

##### Enforcement

???


### <a name="Rt-specialization2"></a>T.67: Use specialization to provide alternative implementations for irregular types

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Rt-cast"></a>T.68: Use `{}` rather than `()` within templates to avoid ambiguities

##### Reason

 `()` is vulnerable to grammar ambiguities.

##### Example

    template<typename T, typename U>
    void f(T t, U u)
    {
        T v1(x);    // is v1 a function of a variable?
        T v2 {x};   // variable
        auto x = T(u);  // construction or cast?
    }

    f(1, "asdf"); // bad: cast from const char* to int

##### Enforcement

* flag `()` initializers
* flag function-style casts


### <a name="Rt-customization"></a>T.69: Inside a template, don't make an unqualified nonmember function call unless you intend it to be a customization point

##### Reason

* Provide only intended flexibility.
* Avoid vulnerability to accidental environmental changes.

##### Example

There are three major ways to let calling code customize a template.

    template<class T>
        // Call a member function
    void test1(T t)
    {
        t.f();    // require T to provide f()
    }

    template<class T>
    void test2(T t)
        // Call a nonmember function without qualification
    {
        f(t);  // require f(/*T*/) be available in caller's scope or in T's namespace
    }

    template<class T>
    void test3(T t)
        // Invoke a "trait"
    {
        test_traits<T>::f(t); // require customizing test_traits<>
                              // to get non-default functions/types
    }

A trait is usually a type alias to compute a type,
a `constexpr` function to compute a value,
or a traditional traits template to be specialized on the user's type.

##### Note

If you intend to call your own helper function `helper(t)` with a value `t` that depends on a template type parameter,
put it in a `::detail` namespace and qualify the call as `detail::helper(t);`.
An unqualified call becomes a customization point where any function `helper` in the namespace of `t`'s type can be invoked;
this can cause problems like [unintentionally invoking unconstrained function templates](#Rt-unconstrained-adl).


##### Enforcement

* In a template, flag an unqualified call to a nonmember function that passes a variable of dependent type when there is a nonmember function of the same name in the template's namespace.


## <a name="SS-temp-hier"></a>T.temp-hier: Template and hierarchy rules:

Templates are the backbone of C++'s support for generic programming and class hierarchies the backbone of its support
for object-oriented programming.
The two language mechanisms can be used effectively in combination, but a few design pitfalls must be avoided.

### <a name="Rt-hier"></a>T.80: Do not naively templatize a class hierarchy

##### Reason

Templating a class hierarchy that has many functions, especially many virtual functions, can lead to code bloat.

##### Example, bad

    template<typename T>
    struct Container {         // an interface
        virtual T* get(int i);
        virtual T* first();
        virtual T* next();
        virtual void sort();
    };

    template<typename T>
    class Vector : public Container<T> {
    public:
        // ...
    };

    Vector<int> vi;
    Vector<string> vs;

It is probably a dumb idea to define a `sort` as a member function of a container, but it is not unheard of and it makes a good example of what not to do.

Given this, the compiler cannot know if `vector<int>::sort()` is called, so it must generate code for it.
Similar for `vector<string>::sort()`.
Unless those two functions are called that's code bloat.
Imagine what this would do to a class hierarchy with dozens of member functions and dozens of derived classes with many instantiations.

##### Note

In many cases you can provide a stable interface by not parameterizing a base;
see ["stable base"](#Rt-abi) and [OO and GP](#Rt-generic-oo)

##### Enforcement

* Flag virtual functions that depend on a template argument. ??? False positives

### <a name="Rt-array"></a>T.81: Do not mix hierarchies and arrays

##### Reason

An array of derived classes can implicitly "decay" to a pointer to a base class with potential disastrous results.

##### Example

Assume that `Apple` and `Pear` are two kinds of `Fruit`s.

    void maul(Fruit* p)
    {
        *p = Pear{};     // put a Pear into *p
        p[1] = Pear{};   // put a Pear into p[1]
    }

    Apple aa [] = { an_apple, another_apple };   // aa contains Apples (obviously!)

    maul(aa);
    Apple& a0 = &aa[0];   // a Pear?
    Apple& a1 = &aa[1];   // a Pear?

Probably, `aa[0]` will be a `Pear` (without the use of a cast!).
If `sizeof(Apple) != sizeof(Pear)` the access to `aa[1]` will not be aligned to the proper start of an object in the array.
We have a type violation and possibly (probably) a memory corruption.
Never write such code.

Note that `maul()` violates the a [`T*` points to an individual object rule](#Rf-ptr).

**Alternative**: Use a proper (templatized) container:

    void maul2(Fruit* p)
    {
        *p = Pear{};   // put a Pear into *p
    }

    vector<Apple> va = { an_apple, another_apple };   // va contains Apples (obviously!)

    maul2(va);       // error: cannot convert a vector<Apple> to a Fruit*
    maul2(&va[0]);   // you asked for it

    Apple& a0 = &va[0];   // a Pear?

Note that the assignment in `maul2()` violated the [no-slicing rule](#Res-slice).

##### Enforcement

* Detect this horror!

### <a name="Rt-linear"></a>T.82: Linearize a hierarchy when virtual functions are undesirable

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Rt-virtual"></a>T.83: Do not declare a member function template virtual

##### Reason

C++ does not support that.
If it did, vtbls could not be generated until link time.
And in general, implementations must deal with dynamic linking.

##### Example, don't

    class Shape {
        // ...
        template<class T>
        virtual bool intersect(T* p);   // error: template cannot be virtual
    };

##### Note

We need a rule because people keep asking about this

##### Alternative

Double dispatch, visitors, calculate which function to call

##### Enforcement

The compiler handles that.

### <a name="Rt-abi"></a>T.84: Use a non-template core implementation to provide an ABI-stable interface

##### Reason

Improve stability of code.
Avoid code bloat.

##### Example

It could be a base class:

    struct Link_base {   // stable
        Link_base* suc;
        Link_base* pre;
    };

    template<typename T>   // templated wrapper to add type safety
    struct Link : Link_base {
        T val;
    };

    struct List_base {
        Link_base* first;   // first element (if any)
        int sz;             // number of elements
        void add_front(Link_base* p);
        // ...
    };

    template<typename T>
    class List : List_base {
    public:
        void put_front(const T& e) { add_front(new Link<T>{e}); }   // implicit cast to Link_base
        T& front() { static_cast<Link<T>*>(first).val; }   // explicit cast back to Link<T>
        // ...
    };

    List<int> li;
    List<string> ls;

Now there is only one copy of the operations linking and unlinking elements of a `List`.
The `Link` and `List` classes do nothing but type manipulation.

Instead of using a separate "base" type, another common technique is to specialize for `void` or `void*` and have the general template for `T` be just the safely-encapsulated casts to and from the core `void` implementation.

**Alternative**: Use a [Pimpl](#Ri-pimpl) implementation.

##### Enforcement

???

## <a name="SS-variadic"></a>T.var: Variadic template rules

???

### <a name="Rt-variadic"></a>T.100: Use variadic templates when you need a function that takes a variable number of arguments of a variety of types

##### Reason

Variadic templates is the most general mechanism for that, and is both efficient and type-safe. Don't use C varargs.

##### Example

    ??? printf

##### Enforcement

* Flag uses of `va_arg` in user code.

### <a name="Rt-variadic-pass"></a>T.101: ??? How to pass arguments to a variadic template ???

##### Reason

 ???

##### Example

    ??? beware of move-only and reference arguments

##### Enforcement

???

### <a name="Rt-variadic-process"></a>T.102: How to process arguments to a variadic template

##### Reason

 ???

##### Example

    ??? forwarding, type checking, references

##### Enforcement

???

### <a name="Rt-variadic-not"></a>T.103: Don't use variadic templates for homogeneous argument lists

##### Reason

There are more precise ways of specifying a homogeneous sequence, such as an `initializer_list`.

##### Example

    ???

##### Enforcement

???

## <a name="SS-meta"></a>T.meta: Template metaprogramming (TMP)

Templates provide a general mechanism for compile-time programming.

Metaprogramming is programming where at least one input or one result is a type.
Templates offer Turing-complete (modulo memory capacity) duck typing at compile time.
The syntax and techniques needed are pretty horrendous.

### <a name="Rt-metameta"></a>T.120: Use template metaprogramming only when you really need to

##### Reason

Template metaprogramming is hard to get right, slows down compilation, and is often very hard to maintain.
However, there are real-world examples where template metaprogramming provides better performance than any alternative short of expert-level assembly code.
Also, there are real-world examples where template metaprogramming expresses the fundamental ideas better than run-time code.
For example, if you really need AST manipulation at compile time (e.g., for optional matrix operation folding) there may be no other way in C++.

##### Example, bad

    ???

##### Example, bad

    enable_if

Instead, use concepts. But see [How to emulate concepts if you don't have language support](#Rt-emulate).

##### Example

    ??? good

**Alternative**: If the result is a value, rather than a type, use a [`constexpr` function](#Rt-fct).

##### Note

If you feel the need to hide your template metaprogramming in macros, you have probably gone too far.

### <a name="Rt-emulate"></a>T.121: Use template metaprogramming primarily to emulate concepts

##### Reason

Until concepts become generally available, we need to emulate them using TMP.
Use cases that require concepts (e.g. overloading based on concepts) are among the most common (and simple) uses of TMP.

##### Example

    template<typename Iter>
        /*requires*/ enable_if<random_access_iterator<Iter>, void>
    advance(Iter p, int n) { p += n; }

    template<typename Iter>
        /*requires*/ enable_if<forward_iterator<Iter>, void>
    advance(Iter p, int n) { assert(n >= 0); while (n--) ++p;}

##### Note

Such code is much simpler using concepts:

    void advance(RandomAccessIterator p, int n) { p += n; }

    void advance(ForwardIterator p, int n) { assert(n >= 0); while (n--) ++p;}

##### Enforcement

???

### <a name="Rt-tmp"></a>T.122: Use templates (usually template aliases) to compute types at compile time

##### Reason

Template metaprogramming is the only directly supported and half-way principled way of generating types at compile time.

##### Note

"Traits" techniques are mostly replaced by template aliases to compute types and `constexpr` functions to compute values.

##### Example

    ??? big object / small object optimization

##### Enforcement

???

### <a name="Rt-fct"></a>T.123: Use `constexpr` functions to compute values at compile time

##### Reason

A function is the most obvious and conventional way of expressing the computation of a value.
Often a `constexpr` function implies less compile-time overhead than alternatives.

##### Note

"Traits" techniques are mostly replaced by template aliases to compute types and `constexpr` functions to compute values.

##### Example

    template<typename T>
        // requires Number<T>
    constexpr T pow(T v, int n)   // power/exponential
    {
        T res = 1;
        while (n--) res *= v;
        return res;
    }

    constexpr auto f7 = pow(pi, 7);

##### Enforcement

* Flag template metaprograms yielding a value. These should be replaced with `constexpr` functions.

### <a name="Rt-std-tmp"></a>T.124: Prefer to use standard-library TMP facilities

##### Reason

Facilities defined in the standard, such as `conditional`, `enable_if`, and `tuple`, are portable and can be assumed to be known.

##### Example

    ???

##### Enforcement

???

### <a name="Rt-lib"></a>T.125: If you need to go beyond the standard-library TMP facilities, use an existing library

##### Reason

Getting advanced TMP facilities is not easy and using a library makes you part of a (hopefully supportive) community.
Write your own "advanced TMP support" only if you really have to.

##### Example

    ???

##### Enforcement

???

## <a name="SS-temp-other"></a>Other template rules

### <a name="Rt-name"></a>T.140: Name all operations with potential for reuse

##### Reason

Documentation, readability, opportunity for reuse.

##### Example

    struct Rec {
        string name;
        string addr;
        int id;         // unique identifier
    };

    bool same(const Rec& a, const Rec& b)
    {
        return a.id == b.id;
    }

    vector<Rec*> find_id(const string& name);    // find all records for "name"

    auto x = find_if(vr.begin(), vr.end(),
        [&](Rec& r) {
            if (r.name.size() != n.size()) return false; // name to compare to is in n
            for (int i = 0; i < r.name.size(); ++i)
                if (tolower(r.name[i]) != tolower(n[i])) return false;
            return true;
        }
    );

There is a useful function lurking here (case insensitive string comparison), as there often is when lambda arguments get large.

    bool compare_insensitive(const string& a, const string& b)
    {
        if (a.size() != b.size()) return false;
        for (int i = 0; i < a.size(); ++i) if (tolower(a[i]) != tolower(b[i])) return false;
        return true;
    }

    auto x = find_if(vr.begin(), vr.end(),
        [&](Rec& r) { compare_insensitive(r.name, n); }
    );

Or maybe (if you prefer to avoid the implicit name binding to n):

    auto cmp_to_n = [&n](const string& a) { return compare_insensitive(a, n); };

    auto x = find_if(vr.begin(), vr.end(),
        [](const Rec& r) { return cmp_to_n(r.name); }
    );

##### Note

whether functions, lambdas, or operators.

##### Exception

* Lambdas logically used only locally, such as an argument to `for_each` and similar control flow algorithms.
* Lambdas as [initializers](#???)

##### Enforcement

* (hard) flag similar lambdas
* ???

### <a name="Rt-lambda"></a>T.141: Use an unnamed lambda if you need a simple function object in one place only

##### Reason

That makes the code concise and gives better locality than alternatives.

##### Example

    auto earlyUsersEnd = std::remove_if(users.begin(), users.end(),
                                        [](const User &a) { return a.id > 100; });


##### Exception

Naming a lambda can be useful for clarity even if it is used only once.

##### Enforcement

* Look for identical and near identical lambdas (to be replaced with named functions or named lambdas).

### <a name="Rt-var"></a>T.142?: Use template variables to simplify notation

##### Reason

Improved readability.

##### Example

    ???

##### Enforcement

???

### <a name="Rt-nongeneric"></a>T.143: Don't write unintentionally nongeneric code

##### Reason

Generality. Reusability. Don't gratuitously commit to details; use the most general facilities available.

##### Example

Use `!=` instead of `<` to compare iterators; `!=` works for more objects because it doesn't rely on ordering.

    for (auto i = first; i < last; ++i) {   // less generic
        // ...
    }

    for (auto i = first; i != last; ++i) {   // good; more generic
        // ...
    }

Of course, range-`for` is better still where it does what you want.

##### Example

Use the least-derived class that has the functionality you need.

    class Base {
    public:
        Bar f();
        Bar g();
    };

    class Derived1 : public Base {
    public:
        Bar h();
    };

    class Derived2 : public Base {
    public:
        Bar j();
    };

    // bad, unless there is a specific reason for limiting to Derived1 objects only
    void my_func(Derived1& param)
    {
        use(param.f());
        use(param.g());
    }

    // good, uses only Base interface so only commit to that
    void my_func(Base& param)
    {
        use(param.f());
        use(param.g());
    }

##### Enforcement

* Flag comparison of iterators using `<` instead of `!=`.
* Flag `x.size() == 0` when `x.empty()` or `x.is_empty()` is available. Emptiness works for more containers than size(), because some containers don't know their size or are conceptually of unbounded size.
* Flag functions that take a pointer or reference to a more-derived type but only use functions declared in a base type.

### <a name="Rt-specialize-function"></a>T.144: Don't specialize function templates

##### Reason

You can't partially specialize a function template per language rules. You can fully specialize a function template but you almost certainly want to overload instead -- because function template specializations don't participate in overloading, they don't act as you probably wanted. Rarely, you should actually specialize by delegating to a class template that you can specialize properly.

##### Example

    ???

**Exceptions**: If you do have a valid reason to specialize a function template, just write a single function template that delegates to a class template, then specialize the class template (including the ability to write partial specializations).

##### Enforcement

* Flag all specializations of a function template. Overload instead.


### <a name="Rt-check-class"></a>T.150: Check that a class matches a concept using `static_assert`

##### Reason

If you intend for a class to match a concept, verifying that early saves users pain.

##### Example

    class X {
    public:
        X() = delete;
        X(const X&) = default;
        X(X&&) = default;
        X& operator=(const X&) = default;
        // ...
    };

Somewhere, possibly in an implementation file, let the compiler check the desired properties of `X`:

    static_assert(Default_constructible<X>);    // error: X has no default constructor
    static_assert(Copyable<X>);                 // error: we forgot to define X's move constructor


##### Enforcement

Not feasible.

# <a name="S-cpl"></a>CPL: C-style programming

C and C++ are closely related languages.
They both originate in "Classic C" from 1978 and have evolved in ISO committees since then.
Many attempts have been made to keep them compatible, but neither is a subset of the other.

C rule summary:

* [CPL.1: Prefer C++ to C](#Rcpl-C)
* [CPL.2: If you must use C, use the common subset of C and C++, and compile the C code as C++](#Rcpl-subset)
* [CPL.3: If you must use C for interfaces, use C++ in the calling code using such interfaces](#Rcpl-interface)

### <a name="Rcpl-C"></a>CPL.1: Prefer C++ to C

##### Reason

C++ provides better type checking and more notational support.
It provides better support for high-level programming and often generates faster code.

##### Example

    char ch = 7;
    void* pv = &ch;
    int* pi = pv;   // not C++
    *pi = 999;      // overwrite sizeof(int) bytes near &ch

The rules for implicit casting to and from `void*` in C are subtle and unenforced.
In particular, this example violates a rule against converting to a type with stricter alignment.

##### Enforcement

Use a C++ compiler.

### <a name="Rcpl-subset"></a>CPL.2: If you must use C, use the common subset of C and C++, and compile the C code as C++

##### Reason

That subset can be compiled with both C and C++ compilers, and when compiled as C++ is better type checked than "pure C."

##### Example

    int* p1 = malloc(10 * sizeof(int));                      // not C++
    int* p2 = static_cast<int*>(malloc(10 * sizeof(int)));   // not C, C-style C++
    int* p3 = new int[10];                                   // not C
    int* p4 = (int*) malloc(10 * sizeof(int));               // both C and C++

##### Enforcement

* Flag if using a build mode that compiles code as C.

  * The C++ compiler will enforce that the code is valid C++ unless you use C extension options.

### <a name="Rcpl-interface"></a>CPL.3: If you must use C for interfaces, use C++ in the calling code using such interfaces

##### Reason

C++ is more expressive than C and offers better support for many types of programming.

##### Example

For example, to use a 3rd party C library or C systems interface, define the low-level interface in the common subset of C and C++ for better type checking.
Whenever possible encapsulate the low-level interface in an interface that follows the C++ guidelines (for better abstraction, memory safety, and resource safety) and use that C++ interface in C++ code.

##### Example

You can call C from C++:

    // in C:
    double sqrt(double);

    // in C++:
    extern "C" double sqrt(double);

    sqrt(2);

##### Example

You can call C++ from C:

    // in C:
    X call_f(struct Y*, int);

    // in C++:
    extern "C" X call_f(Y* p, int i)
    {
        return p->f(i);   // possibly a virtual function call
    }

##### Enforcement

None needed

# <a name="S-source"></a>SF: Source files

Distinguish between declarations (used as interfaces) and definitions (used as implementations).
Use header files to represent interfaces and to emphasize logical structure.

Source file rule summary:

* [SF.1: Use a `.cpp` suffix for code files and `.h` for interface files if your project doesn't already follow another convention](#Rs-file-suffix)
* [SF.2: A `.h` file may not contain object definitions or non-inline function definitions](#Rs-inline)
* [SF.3: Use `.h` files for all declarations used in multiple source files](#Rs-declaration-header)
* [SF.4: Include `.h` files before other declarations in a file](#Rs-include-order)
* [SF.5: A `.cpp` file must include the `.h` file(s) that defines its interface](#Rs-consistency)
* [SF.6: Use `using namespace` directives for transition, for foundation libraries (such as `std`), or within a local scope (only)](#Rs-using)
* [SF.7: Don't write `using namespace` at global scope in a header file](#Rs-using-directive)
* [SF.8: Use `#include` guards for all `.h` files](#Rs-guards)
* [SF.9: Avoid cyclic dependencies among source files](#Rs-cycles)
* [SF.10: Avoid dependencies on implicitly `#include`d names](#Rs-implicit)
* [SF.11: Header files should be self-contained](#Rs-contained)

* [SF.20: Use `namespace`s to express logical structure](#Rs-namespace)
* [SF.21: Don't use an unnamed (anonymous) namespace in a header](#Rs-unnamed)
* [SF.22: Use an unnamed (anonymous) namespace for all internal/nonexported entities](#Rs-unnamed2)

### <a name="Rs-file-suffix"></a>SF.1: Use a `.cpp` suffix for code files and `.h` for interface files if your project doesn't already follow another convention

##### Reason

It's a longstanding convention.
But consistency is more important, so if your project uses something else, follow that.

##### Note

This convention reflects a common use pattern:
Headers are more often shared with C to compile as both C++ and C, which typically uses `.h`,
and it's easier to name all headers `.h` instead of having different extensions for just those headers that are intended to be shared with C.
On the other hand, implementation files are rarely shared with C and so should typically be distinguished from `.c` files,
so it's normally best to name all C++ implementation files something else (such as `.cpp`).

The specific names `.h` and `.cpp` are not required (just recommended as a default) and other names are in widespread use.
Examples are `.hh`, `.C`, and `.cxx`. Use such names equivalently.
In this document, we refer to `.h` and `.cpp` as a shorthand for header and implementation files,
even though the actual extension may be different.

Your IDE (if you use one) may have strong opinions about suffixes.

##### Example

    // foo.h:
    extern int a;   // a declaration
    extern void foo();

    // foo.cpp:
    int a;   // a definition
    void foo() { ++a; }

`foo.h` provides the interface to `foo.cpp`. Global variables are best avoided.

##### Example, bad

    // foo.h:
    int a;   // a definition
    void foo() { ++a; }

`#include <foo.h>` twice in a program and you get a linker error for two one-definition-rule violations.

##### Enforcement

* Flag non-conventional file names.
* Check that `.h` and `.cpp` (and equivalents) follow the rules below.

### <a name="Rs-inline"></a>SF.2: A `.h` file may not contain object definitions or non-inline function definitions

##### Reason

Including entities subject to the one-definition rule leads to linkage errors.

##### Example

    // file.h:
    namespace Foo {
        int x = 7;
        int xx() { return x+x; }
    }

    // file1.cpp:
    #include <file.h>
    // ... more ...

     // file2.cpp:
    #include <file.h>
    // ... more ...

Linking `file1.cpp` and `file2.cpp` will give two linker errors.

**Alternative formulation**: A `.h` file must contain only:

* `#include`s of other `.h` files (possibly with include guards)
* templates
* class definitions
* function declarations
* `extern` declarations
* `inline` function definitions
* `constexpr` definitions
* `const` definitions
* `using` alias definitions
* ???

##### Enforcement

Check the positive list above.

### <a name="Rs-declaration-header"></a>SF.3: Use `.h` files for all declarations used in multiple source files

##### Reason

Maintainability. Readability.

##### Example, bad

    // bar.cpp:
    void bar() { cout << "bar\n"; }

    // foo.cpp:
    extern void bar();
    void foo() { bar(); }

A maintainer of `bar` cannot find all declarations of `bar` if its type needs changing.
The user of `bar` cannot know if the interface used is complete and correct. At best, error messages come (late) from the linker.

##### Enforcement

* Flag declarations of entities in other source files not placed in a `.h`.

### <a name="Rs-include-order"></a>SF.4: Include `.h` files before other declarations in a file

##### Reason

Minimize context dependencies and increase readability.

##### Example

    #include <vector>
    #include <algorithm>
    #include <string>

    // ... my code here ...

##### Example, bad

    #include <vector>

    // ... my code here ...

    #include <algorithm>
    #include <string>

##### Note

This applies to both `.h` and `.cpp` files.

##### Note

There is an argument for insulating code from declarations and macros in header files by `#including` headers *after* the code we want to protect
(as in the example labeled "bad").
However

* that only works for one file (at one level): Use that technique in a header included with other headers and the vulnerability reappears.
* a namespace (an "implementation namespace") can protect against many context dependencies.
* full protection and flexibility require modules.

**See also**:

* [Working Draft, Extensions to C++ for Modules](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4592.pdf)
* [Modules, Componentization, and Transition](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0141r0.pdf)

##### Enforcement

Easy.

### <a name="Rs-consistency"></a>SF.5: A `.cpp` file must include the `.h` file(s) that defines its interface

##### Reason

This enables the compiler to do an early consistency check.

##### Example, bad

    // foo.h:
    void foo(int);
    int bar(long);
    int foobar(int);

    // foo.cpp:
    void foo(int) { /* ... */ }
    int bar(double) { /* ... */ }
    double foobar(int);

The errors will not be caught until link time for a program calling `bar` or `foobar`.

##### Example

    // foo.h:
    void foo(int);
    int bar(long);
    int foobar(int);

    // foo.cpp:
    #include <foo.h>

    void foo(int) { /* ... */ }
    int bar(double) { /* ... */ }
    double foobar(int);   // error: wrong return type

The return-type error for `foobar` is now caught immediately when `foo.cpp` is compiled.
The argument-type error for `bar` cannot be caught until link time because of the possibility of overloading, but systematic use of `.h` files increases the likelihood that it is caught earlier by the programmer.

##### Enforcement

???

### <a name="Rs-using"></a>SF.6: Use `using namespace` directives for transition, for foundation libraries (such as `std`), or within a local scope (only)

##### Reason

 `using namespace` can lead to name clashes, so it should be used sparingly.
 However, it is not always possible to qualify every name from a namespace in user code (e.g., during transition)
 and sometimes a namespace is so fundamental and prevalent in a code base, that consistent qualification would be verbose and distracting.

##### Example

    #include <string>
    #include <vector>
    #include <iostream>
    #include <memory>
    #include <algorithm>

    using namespace std;

    // ...

Here (obviously), the standard library is used pervasively and apparently no other library is used, so requiring `std::` everywhere
could be distracting.

##### Example

The use of `using namespace std;` leaves the programmer open to a name clash with a name from the standard library

    #include <cmath>
    using namespace std;

    int g(int x)
    {
        int sqrt = 7;
        // ...
        return sqrt(x); // error
    }

However, this is not particularly likely to lead to a resolution that is not an error and
people who use `using namespace std` are supposed to know about `std` and about this risk.

##### Note

A `.cpp` file is a form of local scope.
There is little difference in the opportunities for name clashes in an N-line `.cpp` containing a `using namespace X`,
an N-line function containing a `using namespace X`,
and M functions each containing a `using namespace X`with N lines of code in total.

##### Note

[Don't write `using namespace` in a header file](#Rs-using-directive).

##### Enforcement

Flag multiple `using namespace` directives for different namespaces in a single source file.

### <a name="Rs-using-directive"></a>SF.7: Don't write `using namespace` at global scope in a header file

##### Reason

Doing so takes away an `#include`r's ability to effectively disambiguate and to use alternatives. It also makes `#include`d headers order-dependent as they may have different meaning when included in different orders.

##### Example

    // bad.h
    #include <iostream>
    using namespace std; // bad

    // user.cpp
    #include "bad.h"

    bool copy(/*... some parameters ...*/);    // some function that happens to be named copy

    int main() {
        copy(/*...*/);    // now overloads local ::copy and std::copy, could be ambiguous
    }

##### Enforcement

Flag `using namespace` at global scope in a header file.

### <a name="Rs-guards"></a>SF.8: Use `#include` guards for all `.h` files

##### Reason

To avoid files being `#include`d several times.

In order to avoid include guard collisions, do not just name the guard after the filename.
Be sure to also include a key and good differentiator, such as the name of library or component
the header file is part of.

##### Example

    // file foobar.h:
    #ifndef LIBRARY_FOOBAR_H
    #define LIBRARY_FOOBAR_H
    // ... declarations ...
    #endif // LIBRARY_FOOBAR_H

##### Enforcement

Flag `.h` files without `#include` guards.

##### Note

Some implementations offer vendor extensions like `#pragma once` as alternative to include guards.
It is not standard and it is not portable.  It injects the hosting machine's filesystem semantics
into your program, in addition to locking you down to a vendor.
Our recommendation is to write in ISO C++: See [rule P.2](#Rp-Cplusplus).

### <a name="Rs-cycles"></a>SF.9: Avoid cyclic dependencies among source files

##### Reason

Cycles complicates comprehension and slows down compilation.
Complicates conversion to use language-supported modules (when they become available).

##### Note

Eliminate cycles; don't just break them with `#include` guards.

##### Example, bad

    // file1.h:
    #include "file2.h"

    // file2.h:
    #include "file3.h"

    // file3.h:
    #include "file1.h"

##### Enforcement

Flag all cycles.


### <a name="Rs-implicit"></a>SF.10: Avoid dependencies on implicitly `#include`d names

##### Reason

Avoid surprises.
Avoid having to change `#include`s if an `#include`d header changes.
Avoid accidentally becoming dependent on implementation details and logically separate entities included in a header.

##### Example

    #include <iostream>
    using namespace std;

    void use()                  // bad
    {
        string s;
        cin >> s;               // fine
        getline(cin, s);        // error: getline() not defined
        if (s == "surprise") {  // error == not defined
            // ...
        }
    }

`<iostream>` exposes the definition of `std::string` ("why?" makes for a fun trivia question),
but it is not required to do so by transitively including the entire `<string>` header,
resulting in the popular beginner question "why doesn't `getline(cin,s);` work?"
or even an occasional "`string`s cannot be compared with `==`).

The solution is to explicitly `#include <string>`:

    #include <iostream>
    #include <string>
    using namespace std;

    void use()
    {
        string s;
        cin >> s;               // fine
        getline(cin, s);        // fine
        if (s == "surprise") {  // fine
            // ...
        }
    }

##### Note

Some headers exist exactly to collect a set of consistent declarations from a variety of headers.
For example:

    // basic_std_lib.h:

    #include <vector>
    #include <string>
    #include <map>
    #include <iostream>
    #include <random>
    #include <vector>

a user can now get that set of declarations with a single `#include`"

    #include "basic_std_lib.h"

This rule against implicit inclusion is not meant to prevent such deliberate aggregation.

##### Enforcement

Enforcement would require some knowledge about what in a header is meant to be "exported" to users and what is there to enable implementation.
No really good solution is possible until we have modules.

### <a name="Rs-contained"></a>SF.11: Header files should be self-contained

##### Reason

Usability, headers should be simple to use and work when included on their own.
Headers should encapsulate the functionality they provide.
Avoid clients of a header having to manage that header's dependencies.

##### Example

    #include "helpers.h"
    // helpers.h depends on std::string and includes <string>

##### Note

Failing to follow this results in difficult to diagnose errors for clients of a header.

##### Enforcement

A test should verify that the header file itself compiles or that a cpp file which only includes the header file compiles.

### <a name="Rs-namespace"></a>SF.20: Use `namespace`s to express logical structure

##### Reason

 ???

##### Example

    ???

##### Enforcement

???

### <a name="Rs-unnamed"></a>SF.21: Don't use an unnamed (anonymous) namespace in a header

##### Reason

It is almost always a bug to mention an unnamed namespace in a header file.

##### Example

    ???

##### Enforcement

* Flag any use of an anonymous namespace in a header file.

### <a name="Rs-unnamed2"></a>SF.22: Use an unnamed (anonymous) namespace for all internal/nonexported entities

##### Reason

Nothing external can depend on an entity in a nested unnamed namespace.
Consider putting every definition in an implementation source file in an unnamed namespace unless that is defining an "external/exported" entity.

##### Example

An API class and its members can't live in an unnamed namespace; but any "helper" class or function that is defined in an implementation source file should be at an unnamed namespace scope.

    ???

##### Enforcement

* ???

# <a name="S-stdlib"></a>SL: The Standard Library

Using only the bare language, every task is tedious (in any language).
Using a suitable library any task can be reasonably simple.

The standard library has steadily grown over the years.
Its description in the standard is now larger than that of the language features.
So, it is likely that this library section of the guidelines will eventually grow in size to equal or exceed all the rest.

<< ??? We need another level of rule numbering ??? >>

C++ Standard Library component summary:

* [SL.con: Containers](#SS-con)
* [SL.str: String](#SS-string)
* [SL.io: Iostream](#SS-io)
* [SL.regex: Regex](#SS-regex)
* [SL.chrono: Time](#SS-chrono)
* [SL.C: The C Standard Library](#SS-clib)

Standard-library rule summary:

* [SL.1: Use libraries wherever possible](#Rsl-lib)
* [SL.2: Prefer the standard library to other libraries](#Rsl-sl)
* [SL.3: Do not add non-standard entities to namespace `std`](#sl-std)
* [SL.4: Use the standard library in a type-safe manner](#sl-safe)
* ???

### <a name="Rsl-lib"></a>SL.1:  Use libraries wherever possible

##### Reason

Save time. Don't re-invent the wheel.
Don't replicate the work of others.
Benefit from other people's work when they make improvements.
Help other people when you make improvements.

### <a name="Rsl-sl"></a>SL.2: Prefer the standard library to other libraries

##### Reason

More people know the standard library.
It is more likely to be stable, well-maintained, and widely available than your own code or most other libraries.


### <a name="sl-std"></a>SL.3: Do not add non-standard entities to namespace `std`

##### Reason

Adding to `std` may change the meaning of otherwise standards conforming code.
Additions to `std` may clash with future versions of the standard.

##### Example

    ???

##### Enforcement

Possible, but messy and likely to cause problems with platforms.

### <a name="sl-safe"></a>SL.4: Use the standard library in a type-safe manner

##### Reason

Because, obviously, breaking this rule can lead to undefined behavior, memory corruption, and all kinds of other bad errors.

##### Note

This is a semi-philosophical meta-rule, which needs many supporting concrete rules.
We need it as an umbrella for the more specific rules.

Summary of more specific rules:

* [SL.4: Use the standard library in a type-safe manner](#sl-safe)


## <a name="SS-con"></a>SL.con: Containers

???

Container rule summary:

* [SL.con.1: Prefer using STL `array` or `vector` instead of a C array](#Rsl-arrays)
* [SL.con.2: Prefer using STL `vector` by default unless you have a reason to use a different container](#Rsl-vector)
* [SL.con.3: Avoid bounds errors](#Rsl-bounds)
*  ???

### <a name="Rsl-arrays"></a>SL.con.1: Prefer using STL `array` or `vector` instead of a C array

##### Reason

C arrays are less safe, and have no advantages over `array` and `vector`.
For a fixed-length array, use `std::array`, which does not degenerate to a pointer when passed to a function and does know its size.
Also, like a built-in array, a stack-allocated `std::array` keeps its elements on the stack.
For a variable-length array, use `std::vector`, which additionally can change its size and handles memory allocation.

##### Example

    int v[SIZE];                        // BAD

    std::array<int, SIZE> w;             // ok

##### Example

    int* v = new int[initial_size];     // BAD, owning raw pointer
    delete[] v;                         // BAD, manual delete

    std::vector<int> w(initial_size);   // ok

##### Note

Use `gsl::span` for non-owning references into a container.

##### Note

Comparing the performance of a fixed-sized array allocated on the stack against a `vector` with its elements on the free store is bogus.
You could just as well compare a `std::array` on the stack against the result of a `malloc()` accessed through a pointer.
For most code, even the difference between stack allocation and free-store allocation doesn't matter, but the convenience and safety of `vector` does.
People working with code for which that difference matters are quite capable of choosing between `array` and `vector`.

##### Enforcement

* Flag declaration of a C array inside a function or class that also declares an STL container (to avoid excessive noisy warnings on legacy non-STL code). To fix: At least change the C array to a `std::array`.

### <a name="Rsl-vector"></a>SL.con.2: Prefer using STL `vector` by default unless you have a reason to use a different container

##### Reason

`vector` and `array` are the only standard containers that offer the following advantages:

* the fastest general-purpose access (random access, including being vectorization-friendly);
* the fastest default access pattern (begin-to-end or end-to-begin is prefetcher-friendly);
* the lowest space overhead (contiguous layout has zero per-element overhead, which is cache-friendly).

Usually you need to add and remove elements from the container, so use `vector` by default; if you don't need to modify the container's size, use `array`.

Even when other containers seem more suited, such a `map` for O(log N) lookup performance or a `list` for efficient insertion in the middle, a `vector` will usually still perform better for containers up to a few KB in size.

##### Note

`string` should not be used as a container of individual characters. A `string` is a textual string; if you want a container of characters, use `vector</*char_type*/>` or `array</*char_type*/>` instead.

##### Exceptions

If you have a good reason to use another container, use that instead. For example:

* If `vector` suits your needs but you don't need the container to be variable size, use `array` instead.

* If you want a dictionary-style lookup container that guarantees O(K) or O(log N) lookups, the container will be larger (more than a few KB) and you perform frequent inserts so that the overhead of maintaining a sorted `vector` is infeasible, go ahead and use an `unordered_map` or `map` instead.

##### Note

To initialize a vector with a number of elements, use `()`-initialization.
To initialize a vector with a list of elements, use `{}`-initialization.

    vector<int> v1(20);  // v1 has 20 elements with the value 0 (vector<int>{})
    vector<int> v2 {20}; // v2 has 1 element with the value 20

[Prefer the {}-initializer syntax](#Res-list).

##### Enforcement

* Flag a `vector` whose size never changes after construction (such as because it's `const` or because no non-`const` functions are called on it). To fix: Use an `array` instead.

### <a name="Rsl-bounds"></a>SL.con.3: Avoid bounds errors

##### Reason

Read or write beyond an allocated range of elements typically leads to bad errors, wrong results, crashes, and security violations.

##### Note

The standard-library functions that apply to ranges of elements all have (or could have) bounds-safe overloads that take `span`.
Standard types such as `vector` can be modified to perform bounds-checks under the bounds profile (in a compatible way, such as by adding contracts), or used with `at()`.

Ideally, the in-bounds guarantee should be statically enforced.
For example:

* a range-`for` cannot loop beyond the range of the container to which it is applied
* a `v.begin(),v.end()` is easily determined to be bounds safe

Such loops are as fast as any unchecked/unsafe equivalent.

Often a simple pre-check can eliminate the need for checking of individual indices.
For example

* for `v.begin(),v.begin()+i` the `i` can easily be checked against `v.size()`

Such loops can be much faster than individually checked element accesses.

##### Example, bad

    void f()
    {
        array<int, 10> a, b;
        memset(a.data(), 0, 10);         // BAD, and contains a length error (length = 10 * sizeof(int))
        memcmp(a.data(), b.data(), 10);  // BAD, and contains a length error (length = 10 * sizeof(int))
    }

Also, `std::array<>::fill()` or `std::fill()` or even an empty initializer are better candidate than `memset()`.

##### Example, good

    void f()
    {
        array<int, 10> a, b, c{};       // c is initialized to zero
        a.fill(0);
        fill(b.begin(), b.end(), 0);    // std::fill()
        fill(b, 0);                     // std::fill() + Ranges TS

        if ( a == b ) {
          // ...
        }
    }

##### Example

If code is using an unmodified standard library, then there are still workarounds that enable use of `std::array` and `std::vector` in a bounds-safe manner. Code can call the `.at()` member function on each class, which will result in an `std::out_of_range` exception being thrown. Alternatively, code can call the `at()` free function, which will result in fail-fast (or a customized action) on a bounds violation.

    void f(std::vector<int>& v, std::array<int, 12> a, int i)
    {
        v[0] = a[0];        // BAD
        v.at(0) = a[0];     // OK (alternative 1)
        at(v, 0) = a[0];    // OK (alternative 2)

        v.at(0) = a[i];     // BAD
        v.at(0) = a.at(i);  // OK (alternative 1)
        v.at(0) = at(a, i); // OK (alternative 2)
    }

##### Enforcement

* Issue a diagnostic for any call to a standard-library function that is not bounds-checked.
??? insert link to a list of banned functions

This rule is part of the [bounds profile](#SS-bounds).

**TODO Notes**:

* Impact on the standard library will require close coordination with WG21, if only to ensure compatibility even if never standardized.
* We are considering specifying bounds-safe overloads for stdlib (especially C stdlib) functions like `memcmp` and shipping them in the GSL.
* For existing stdlib functions and types like `vector` that are not fully bounds-checked, the goal is for these features to be bounds-checked when called from code with the bounds profile on, and unchecked when called from legacy code, possibly using contracts (concurrently being proposed by several WG21 members).



## <a name="SS-string"></a>SL.str: String

Text manipulation is a huge topic.
`std::string` doesn't cover all of it.
This section primarily tries to clarify `std::string`'s relation to `char*`, `zstring`, `string_view`, and `gsl::string_span`.
The important issue of non-ASCII character sets and encodings (e.g., `wchar_t`, Unicode, and UTF-8) will be covered elsewhere.

**See also**: [regular expressions](#SS-regex)

Here, we use "sequence of characters" or "string" to refer to a sequence of characters meant to be read as text (somehow, eventually).
We don't consider

String summary:

* [SL.str.1: Use `std::string` to own character sequences](#Rstr-string)
* [SL.str.2: Use `std::string_view` or `gsl::string_span` to refer to character sequences](#Rstr-view)
* [SL.str.3: Use `zstring` or `czstring` to refer to a C-style, zero-terminated, sequence of characters](#Rstr-zstring)
* [SL.str.4: Use `char*` to refer to a single character](#Rstr-char*)
* [SL.str.5: Use `std::byte` to refer to byte values that do not necessarily represent characters](#Rstr-byte)

* [SL.str.10: Use `std::string` when you need to perform locale-sensitive string operations](#Rstr-locale)
* [SL.str.11: Use `gsl::string_span` rather than `std::string_view` when you need to mutate a string](#Rstr-span)
* [SL.str.12: Use the `s` suffix for string literals meant to be standard-library `string`s](#Rstr-s)

**See also**:

* [F.24 span](#Rf-range)
* [F.25 zstring](#Rf-zstring)


### <a name="Rstr-string"></a>SL.str.1: Use `std::string` to own character sequences

##### Reason

`string` correctly handles allocation, ownership, copying, gradual expansion, and offers a variety of useful operations.

##### Example

    vector<string> read_until(const string& terminator)
    {
        vector<string> res;
        for (string s; cin >> s && s != terminator; ) // read a word
            res.push_back(s);
        return res;
    }

Note how `>>` and `!=` are provided for `string` (as examples of useful operations) and there are no explicit
allocations, deallocations, or range checks (`string` takes care of those).

In C++17, we might use `string_view` as the argument, rather than `const string*` to allow more flexibility to callers:

    vector<string> read_until(string_view terminator)   // C++17
    {
        vector<string> res;
        for (string s; cin >> s && s != terminator; ) // read a word
            res.push_back(s);
        return res;
    }

The `gsl::string_span` is a current alternative offering most of the benefits of `std::string_view` for simple examples:

    vector<string> read_until(string_span terminator)
    {
        vector<string> res;
        for (string s; cin >> s && s != terminator; ) // read a word
            res.push_back(s);
        return res;
    }

##### Example, bad

Don't use C-style strings for operations that require non-trivial memory management

    char* cat(const char* s1, const char* s2)   // beware!
        // return s1 + '.' + s2
    {
        int l1 = strlen(s1);
        int l2 = strlen(s2);
        char* p = (char*) malloc(l1 + l2 + 2);
        strcpy(p, s1, l1);
        p[l1] = '.';
        strcpy(p + l1 + 1, s2, l2);
        p[l1 + l2 + 1] = 0;
        return p;
    }

Did we get that right?
Will the caller remember to `free()` the returned pointer?
Will this code pass a security review?

##### Note

Do not assume that `string` is slower than lower-level techniques without measurement and remember than not all code is performance critical.
[Don't optimize prematurely](#Rper-Knuth)

##### Enforcement

???

### <a name="Rstr-view"></a>SL.str.2: Use `std::string_view` or `gsl::string_span` to refer to character sequences

##### Reason

`std::string_view` or `gsl::string_span` provides simple and (potentially) safe access to character sequences independently of how
those sequences are allocated and stored.

##### Example

    vector<string> read_until(string_span terminator);

    void user(zstring p, const string& s, string_span ss)
    {
        auto v1 = read_until(p);
        auto v2 = read_until(s);
        auto v3 = read_until(ss);
        // ...
    }

##### Note

`std::string_view` (C++17) is read-only.

##### Enforcement

???

### <a name="Rstr-zstring"></a>SL.str.3: Use `zstring` or `czstring` to refer to a C-style, zero-terminated, sequence of characters

##### Reason

Readability.
Statement of intent.
A plain `char*` can be a pointer to a single character, a pointer to an array of characters, a pointer to a C-style (zero-terminated) string, or even to a small integer.
Distinguishing these alternatives prevents misunderstandings and bugs.

##### Example

    void f1(const char* s); // s is probably a string

All we know is that it is supposed to be the nullptr or point to at least one character

    void f1(zstring s);     // s is a C-style string or the nullptr
    void f1(czstring s);    // s is a C-style string constant or the nullptr
    void f1(std::byte* s);  // s is a pointer to a byte (C++17)

##### Note

Don't convert a C-style string to `string` unless there is a reason to.

##### Note

Like any other "plain pointer", a `zstring` should not represent ownership.

##### Note

There are billions of lines of C++ "out there", most use `char*` and `const char*` without documenting intent.
They are used in a wide variety of ways, including to represent ownership and as generic pointers to memory (instead of `void*`).
It is hard to separate these uses, so this guideline is hard to follow.
This is one of the major sources of bugs in C and C++ programs, so it is worthwhile to follow this guideline wherever feasible..

##### Enforcement

* Flag uses of `[]` on a `char*`
* Flag uses of `delete` on a `char*`
* Flag uses of `free()` on a `char*`

### <a name="Rstr-char*"></a>SL.str.4: Use `char*` to refer to a single character

##### Reason

The variety of uses of `char*` in current code is a major source of errors.

##### Example, bad

    char arr[] = {'a', 'b', 'c'};

    void print(const char* p)
    {
        cout << p << '\n';
    }

    void use()
    {
        print(arr);   // run-time error; potentially very bad
    }

The array `arr` is not a C-style string because it is not zero-terminated.

##### Alternative

See [`zstring`](#Rstr-zstring), [`string`](#Rstr-string), and [`string_span`](#Rstr-view).

##### Enforcement

* Flag uses of `[]` on a `char*`

### <a name="Rstr-byte"></a>SL.str.5: Use `std::byte` to refer to byte values that do not necessarily represent characters

##### Reason

Use of `char*` to represent a pointer to something that is not necessarily a character causes confusion
and disables valuable optimizations.

##### Example

    ???

##### Note

C++17

##### Enforcement

???


### <a name="Rstr-locale"></a>SL.str.10: Use `std::string` when you need to perform locale-sensitive string operations

##### Reason

`std::string` supports standard-library [`locale` facilities](#Rstr-locale)

##### Example

    ???

##### Note

???

##### Enforcement

???

### <a name="Rstr-span"></a>SL.str.11: Use `gsl::string_span` rather than `std::string_view` when you need to mutate a string

##### Reason

`std::string_view` is read-only.

##### Example

???

##### Note

???

##### Enforcement

The compiler will flag attempts to write to a `string_view`.

### <a name="Rstr-s"></a>SL.str.12: Use the `s` suffix for string literals meant to be standard-library `string`s

##### Reason

Direct expression of an idea minimizes mistakes.

##### Example

    auto pp1 = make_pair("Tokyo", 9.00);         // {C-style string,double} intended?
    pair<string, double> pp2 = {"Tokyo", 9.00};  // a bit verbose
    auto pp3 = make_pair("Tokyo"s, 9.00);        // {std::string,double}    // C++14
    pair pp4 = {"Tokyo"s, 9.00};                 // {std::string,double}    // C++17



##### Enforcement

???


## <a name="SS-io"></a>SL.io: Iostream

`iostream`s is a type safe, extensible, formatted and unformatted I/O library for streaming I/O.
It supports multiple (and user extensible) buffering strategies and multiple locales.
It can be used for conventional I/O, reading and writing to memory (string streams),
and user-defines extensions, such as streaming across networks (asio: not yet standardized).

Iostream rule summary:

* [SL.io.1: Use character-level input only when you have to](#Rio-low)
* [SL.io.2: When reading, always consider ill-formed input](#Rio-validate)
* [SL.io.3: Prefer iostreams for I/O](#Rio-streams)
* [SL.io.10: Unless you use `printf`-family functions call `ios_base::sync_with_stdio(false)`](#Rio-sync)
* [SL.io.50: Avoid `endl`](#Rio-endl)
* [???](#???)

### <a name="Rio-low"></a>SL.io.1: Use character-level input only when you have to

##### Reason

Unless you genuinely just deal with individual characters, using character-level input leads to the user code performing potentially error-prone
and potentially inefficient composition of tokens out of characters.

##### Example

    char c;
    char buf[128];
    int i = 0;
    while (cin.get(c) && !isspace(c) && i < 128)
        buf[i++] = c;
    if (i == 128) {
        // ... handle too long string ....
    }

Better (much simpler and probably faster):

    string s;
    s.reserve(128);
    cin >> s;

and the `reserve(128)` is probably not worthwhile.

##### Enforcement

???


### <a name="Rio-validate"></a>SL.io.2: When reading, always consider ill-formed input

##### Reason

Errors are typically best handled as soon as possible.
If input isn't validated, every function must be written to cope with bad data (and that is not practical).

##### Example

    ???

##### Enforcement

???

### <a name="Rio-streams"></a>SL.io.3: Prefer `iostream`s for I/O

##### Reason

`iostream`s are safe, flexible, and extensible.

##### Example

    // write a complex number:
    complex<double> z{ 3, 4 };
    cout << z << '\n';

`complex` is a user-defined type and its I/O is defined without modifying the `iostream` library.

##### Example

    // read a file of complex numbers:
    for (complex<double> z; cin >> z; )
        v.push_back(z);

##### Exception

??? performance ???

##### Discussion: `iostream`s vs. the `printf()` family

It is often (and often correctly) pointed out that the `printf()` family has two advantages compared to `iostream`s:
flexibility of formatting and performance.
This has to be weighed against `iostream`s advantages of extensibility to handle user-defined types, resilient against security violations,
implicit memory management, and `locale` handling.

If you need I/O performance, you can almost always do better than `printf()`.

`gets()` `scanf()` using `s`, and `printf()` using `%s` are security hazards (vulnerable to buffer overflow and generally error-prone).
In C11, they are replaced by `gets_s()`, `scanf_s()`, and `printf_s()` as safer alternatives, but they are still not type safe.

##### Enforcement

Optionally flag `<cstdio>` and `<stdio.h>`.

### <a name="Rio-sync"></a>SL.io.10: Unless you use `printf`-family functions call `ios_base::sync_with_stdio(false)`

##### Reason

Synchronizing `iostreams` with `printf-style` I/O can be costly.
`cin` and `cout` are by default synchronized with `printf`.

##### Example

    int main()
    {
        ios_base::sync_with_stdio(false);
        // ... use iostreams ...
    }

##### Enforcement

???

### <a name="Rio-endl"></a>SL.io.50: Avoid `endl`

##### Reason

The `endl` manipulator is mostly equivalent to `'\n'` and `"\n"`;
as most commonly used it simply slows down output by doing redundant `flush()`s.
This slowdown can be significant compared to `printf`-style output.

##### Example

    cout << "Hello, World!" << endl;    // two output operations and a flush
    cout << "Hello, World!\n";          // one output operation and no flush

##### Note

For `cin`/`cout` (and equivalent) interaction, there is no reason to flush; that's done automatically.
For writing to a file, there is rarely a need to `flush`.

##### Note

Apart from the (occasionally important) issue of performance,
the choice between `'\n'` and `endl` is almost completely aesthetic.

## <a name="SS-regex"></a>SL.regex: Regex

`<regex>` is the standard C++ regular expression library.
It supports a variety of regular expression pattern conventions.

## <a name="SS-chrono"></a>SL.chrono: Time

`<chrono>` (defined in namespace `std::chrono`) provides the notions of `time_point` and `duration` together with functions for
outputting time in various units.
It provides clocks for registering `time_points`.

## <a name="SS-clib"></a>SL.C: The C Standard Library

???

C Standard Library rule summary:

* [S.C.1: Don't use setjmp/longjmp](#Rclib-jmp)
* [???](#???)
* [???](#???)

### <a name="Rclib-jmp"></a>SL.C.1: Don't use setjmp/longjmp

##### Reason

a `longjmp` ignores destructors, thus invalidating all resource-management strategies relying on RAII

##### Enforcement

Flag all occurrences of `longjmp`and `setjmp`



# <a name="S-A"></a>A: Architectural ideas

This section contains ideas about higher-level architectural ideas and libraries.

Architectural rule summary:

* [A.1: Separate stable from less stable part of code](#Ra-stable)
* [A.2: Express potentially reusable parts as a library](#Ra-lib)
* [A.4: There should be no cycles among libraries](#?Ra-dag)
* [???](#???)
* [???](#???)
* [???](#???)
* [???](#???)
* [???](#???)
* [???](#???)

### <a name="Ra-stable"></a>A.1: Separate stable from less stable part of code

???

### <a name="Ra-lib"></a>A.2: Express potentially reusable parts as a library

##### Reason

##### Note

A library is a collection of declarations and definitions maintained, documented, and shipped together.
A library could be a set of headers (a "header only library") or a set of headers plus a set of object files.
A library can be statically or dynamically linked into a program, or it may be `#include`d


### <a name="Ra-dag"></a>A.4: There should be no cycles among libraries

##### Reason

* A cycle implies complication of the build process.
* Cycles are hard to understand and may introduce indeterminism (unspecified behavior).

##### Note

A library can contain cyclic references in the definition of its components.
For example:

    ???

However, a library should not depend on another that depends on it.


# <a name="S-not"></a>NR: Non-Rules and myths

This section contains rules and guidelines that are popular somewhere, but that we deliberately don't recommend.
We know full well that there have been times and places where these rules made sense, and we have used them ourselves at times.
However, in the context of the styles of programming we recommend and support with the guidelines, these "non-rules" would do harm.

Even today, there can be contexts where the rules make sense.
For example, lack of suitable tool support can make exceptions unsuitable in hard-real-time systems,
but please don't blindly trust "common wisdom" (e.g., unsupported statements about "efficiency");
such "wisdom" may be based on decades-old information or experienced from languages with very different properties than C++
(e.g., C or Java).

The positive arguments for alternatives to these non-rules are listed in the rules offered as "Alternatives".

Non-rule summary:

* [NR.1: Don't: All declarations should be at the top of a function](#Rnr-top)
* [NR.2: Don't: Have only a single `return`-statement in a function](#Rnr-single-return)
* [NR.3: Don't: Don't use exceptions](#Rnr-no-exceptions)
* [NR.4: Don't: Place each class declaration in its own source file](#Rnr-lots-of-files)
* [NR.5: Don't: Don't do substantive work in a constructor; instead use two-phase initialization](#Rnr-two-phase-init)
* [NR.6: Don't: Place all cleanup actions at the end of a function and `goto exit`](#Rnr-goto-exit)
* [NR.7: Don't: Make all data members `protected`](#Rnr-protected-data)
* ???

### <a name="Rnr-top"></a>NR.1: Don't: All declarations should be at the top of a function

##### Reason (not to follow this rule)

This rule is a legacy of old programming languages that didn't allow initialization of variables and constants after a statement.
This leads to longer programs and more errors caused by uninitialized and wrongly initialized variables.

##### Example, bad

    int use(int x)
    {
        int i;
        char c;
        double d;

        // ... some stuff ...

        if (x < i) {
            // ...
            i = f(x, d);
        }
        if (i < x) {
            // ...
            i = g(x, c);
        }
        return i;
    }

The larger the distance between the uninitialized variable and its use, the larger the chance of a bug.
Fortunately, compilers catch many "used before set" errors.
Unfortunately, compilers cannot catch all such errors and unfortunately, the bugs aren't always as simple to spot as in this small example.


##### Alternative

* [Always initialize an object](#Res-always)
* [ES.21: Don't introduce a variable (or constant) before you need to use it](#Res-introduce)

### <a name="Rnr-single-return"></a>NR.2: Don't: Have only a single `return`-statement in a function

##### Reason (not to follow this rule)

The single-return rule can lead to unnecessarily convoluted code and the introduction of extra state variables.
In particular, the single-return rule makes it harder to concentrate error checking at the top of a function.

##### Example

    template<class T>
    //  requires Number<T>
    string sign(T x)
    {
        if (x < 0)
            return "negative";
        else if (x > 0)
            return "positive";
        return "zero";
    }

to use a single return only we would have to do something like

    template<class T>
    //  requires Number<T>
    string sign(T x)        // bad
    {
        string res;
        if (x < 0)
            res = "negative";
        else if (x > 0)
            res = "positive";
        else
            res = "zero";
        return res;
    }

This is both longer and likely to be less efficient.
The larger and more complicated the function is, the more painful the workarounds get.
Of course many simple functions will naturally have just one `return` because of their simpler inherent logic.

##### Example

    int index(const char* p)
    {
        if (!p) return -1;  // error indicator: alternatively "throw nullptr_error{}"
        // ... do a lookup to find the index for p
        return i;
    }

If we applied the rule, we'd get something like

    int index2(const char* p)
    {
        int i;
        if (!p)
            i = -1;  // error indicator
        else {
            // ... do a lookup to find the index for p
        }
        return i;
    }

Note that we (deliberately) violated the rule against uninitialized variables because this style commonly leads to that.
Also, this style is a temptation to use the [goto exit](#Rnr-goto-exit) non-rule.

##### Alternative

* 保持函数简短
* Feel free to use multiple `return` statements (and to throw exceptions).

### <a name="Rnr-no-exceptions"></a>NR.3: Don't: Don't use exceptions

##### Reason (not to follow this rule)

There seem to be three main reasons given for this non-rule:

* exceptions are inefficient
* exceptions lead to leaks and errors
* exception performance is not predictable

There is no way we can settle this issue to the satisfaction of everybody.
After all, the discussions about exceptions have been going on for 40+ years.
Some languages cannot be used without exceptions, but others do not support them.
This leads to strong traditions for the use and non-use of exceptions, and to heated debates.

However, we can briefly outline why we consider exceptions the best alternative for general-purpose programming
and in the context of these guidelines.
Simple arguments for and against are often inconclusive.
There are specialized applications where exceptions indeed can be inappropriate
(e.g., hard-real-time systems without support for reliable estimates of the cost of handling an exception).

Consider the major objections to exceptions in turn

* Exceptions are inefficient:
Compared to what?
When comparing make sure that the same set of errors are handled and that they are handled equivalently.
In particular, do not compare a program that immediately terminate on seeing an error with a program
that carefully cleans up resources before logging an error.
Yes, some systems have poor exception handling implementations; sometimes, such implementations force us to use
other error-handling approaches, but that's not a fundamental problem with exceptions.
When using an efficiency argument - in any context - be careful that you have good data that actually provides
insight into the problem under discussion.
* Exceptions lead to leaks and errors.
They do not.
If your program is a rat's nest of pointers without an overall strategy for resource management,
you have a problem whatever you do.
If your system consists of a million lines of such code,
you probably will not be able to use exceptions,
but that's a problem with excessive and undisciplined pointer use, rather than with exceptions.
In our opinion, you need RAII to make exception-based error handling simple and safe -- simpler and safer than alternatives.
* Exception performance is not predictable.
If you are in a hard-real-time system where you must guarantee completion of a task in a given time,
you need tools to back up such guarantees.
As far as we know such tools are not available (at least not to most programmers).

Many, possibly most, problems with exceptions stem from historical needs to interact with messy old code.

The fundamental arguments for the use of exceptions are

* They clearly differentiate between erroneous return and ordinary return
* They cannot be forgotten or ignored
* They can be used systematically

Remember

* Exceptions are for reporting errors (in C++; other languages can have different uses for exceptions).
* Exceptions are not for errors that can be handled locally.
* Don't try to catch every exception in every function (that's tedious, clumsy, and leads to slow code).
* Exceptions are not for errors that require instant termination of a module/system after a non-recoverable error.

##### Example

    ???

##### Alternative

* [RAII](#Re-raii)
* Contracts/assertions: Use GSL's `Expects` and `Ensures` (until we get language support for contracts)

### <a name="Rnr-lots-of-files"></a>NR.4: Don't: Place each class declaration in its own source file

##### Reason (not to follow this rule)

The resulting number of files are hard to manage and can slow down compilation.
Individual classes are rarely a good logical unit of maintenance and distribution.

##### Example

    ???

##### Alternative

* Use namespaces containing logically cohesive sets of classes and functions.

### <a name="Rnr-two-phase-init"></a>NR.5: Don't: Don't do substantive work in a constructor; instead use two-phase initialization

##### Reason (not to follow this rule)

Following this rule leads to weaker invariants,
more complicated code (having to deal with semi-constructed objects),
and errors (when we didn't deal correctly with semi-constructed objects consistently).

##### Example

    ???

##### Alternative

* Always establish a class invariant in a constructor.
* Don't define an object before it is needed.

### <a name="Rnr-goto-exit"></a>NR.6: Don't: Place all cleanup actions at the end of a function and `goto exit`

##### Reason (not to follow this rule)

`goto` is error-prone.
This technique is a pre-exception technique for RAII-like resource and error handling.

##### Example, bad

    void do_something(int n)
    {
        if (n < 100) goto exit;
        // ...
        int* p = (int*) malloc(n);
        // ...
        if (some_error) goto_exit;
        // ...
    exit:
        free(p);
    }

and spot the bug.

##### Alternative

* Use exceptions and [RAII](#Re-raii)
* for non-RAII resources, use [`finally`](#Re-finally).

### <a name="Rnr-protected-data"></a>NR.7: Don't: Make all data members `protected`

##### Reason (not to follow this rule)

`protected` data is a source of errors.
`protected` data can be manipulated from an unbounded amount of code in various places.
`protected` data is the class hierarchy equivalent to global data.

##### Example

    ???

##### Alternative

* [Make member data `public` or (preferably) `private`](#Rh-protected)


# <a name="S-references"></a>RF: References

Many coding standards, rules, and guidelines have been written for C++, and especially for specialized uses of C++.
Many

* focus on lower-level issues, such as the spelling of identifiers
* are written by C++ novices
* see "stopping programmers from doing unusual things" as their primary aim
* aim at portability across many compilers (some 10 years old)
* are written to preserve decades old code bases
* aim at a single application domain
* are downright counterproductive
* are ignored (must be ignored by programmers to get their work done well)

A bad coding standard is worse than no coding standard.
However an appropriate set of guidelines are much better than no standards: "Form is liberating."

Why can't we just have a language that allows all we want and disallows all we don't want ("a perfect language")?
Fundamentally, because affordable languages (and their tool chains) also serve people with needs that differ from yours and serve more needs than you have today.
Also, your needs change over time and a general-purpose language is needed to allow you to adapt.
A language that is ideal for today would be overly restrictive tomorrow.

Coding guidelines adapt the use of a language to specific needs.
Thus, there cannot be a single coding style for everybody.
We expect different organizations to provide additions, typically with more restrictions and firmer style rules.

Reference sections:

* [RF.rules: Coding rules](#SS-rules)
* [RF.books: Books with coding guidelines](#SS-books)
* [RF.C++: C++ Programming (C++11/C++14/C++17)](#SS-Cplusplus)
* [RF.web: Websites](#SS-web)
* [RS.video: Videos about "modern C++"](#SS-vid)
* [RF.man: Manuals](#SS-man)
* [RF.core: Core Guidelines materials](#SS-core)

## <a name="SS-rules"></a>RF.rules: Coding rules

* [Boost Library Requirements and Guidelines](http://www.boost.org/development/requirements.html).
  ???.
* [Bloomberg: BDE C++ Coding](https://github.com/bloomberg/bde/wiki/CodingStandards.pdf).
  Has a strong emphasis on code organization and layout.
* Facebook: ???
* [GCC Coding Conventions](https://gcc.gnu.org/codingconventions.html).
  C++03 and (reasonably) a bit backwards looking.
* [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html).
  Geared toward C++03 and (also) older code bases. Google experts are now actively collaborating here on helping to improve these Guidelines, and hopefully to merge efforts so these can be a modern common set they could also recommend.
* [JSF++: JOINT STRIKE FIGHTER AIR VEHICLE C++ CODING STANDARDS](http://www.stroustrup.com/JSF-AV-rules.pdf).
  Document Number 2RDU00001 Rev C. December 2005.
  For flight control software.
  For hard-real-time.
  This means that it is necessarily very restrictive ("if the program fails somebody dies").
  For example, no free store allocation or deallocation may occur after the plane takes off (no memory overflow and no fragmentation allowed).
  No exception may be used (because there was no available tool for guaranteeing that an exception would be handled within a fixed short time).
  Libraries used have to have been approved for mission critical applications.
  Any similarities to this set of guidelines are unsurprising because Bjarne Stroustrup was an author of JSF++.
  Recommended, but note its very specific focus.
* [Mozilla Portability Guide](https://developer.mozilla.org/en-US/docs/Mozilla/C%2B%2B_Portability_Guide).
  As the name indicates, this aims for portability across many (old) compilers.
  As such, it is restrictive.
* [Geosoft.no: C++ Programming Style Guidelines](http://geosoft.no/development/cppstyle.html).
  ???.
* [Possibility.com: C++ Coding Standard](http://www.possibility.com/Cpp/CppCodingStandard.html).
  ???.
* [SEI CERT: Secure C++ Coding Standard](https://www.securecoding.cert.org/confluence/pages/viewpage.action?pageId=637).
  A very nicely done set of rules (with examples and rationales) done for security-sensitive code.
  Many of their rules apply generally.
* [High Integrity C++ Coding Standard](http://www.codingstandard.com/).
* [llvm](http://llvm.org/docs/CodingStandards.html).
  Somewhat brief, pre-C++11, and (not unreasonably) adjusted to its domain.
* ???

## <a name="SS-books"></a>RF.books: Books with coding guidelines

* [Meyers96](#Meyers96) Scott Meyers: *More Effective C++*. Addison-Wesley 1996.
* [Meyers97](#Meyers97) Scott Meyers: *Effective C++, Second Edition*. Addison-Wesley 1997.
* [Meyers01](#Meyers01) Scott Meyers: *Effective STL*. Addison-Wesley 2001.
* [Meyers05](#Meyers05) Scott Meyers: *Effective C++, Third Edition*. Addison-Wesley 2005.
* [Meyers15](#Meyers15) Scott Meyers: *Effective Modern C++*. O'Reilly 2015.
* [SuttAlex05](#SuttAlex05) Sutter and Alexandrescu: *C++ Coding Standards*. Addison-Wesley 2005. More a set of meta-rules than a set of rules. Pre-C++11.
* [Stroustrup05](#Stroustrup05) Bjarne Stroustrup: [A rationale for semantically enhanced library languages](http://www.stroustrup.com/SELLrationale.pdf).
  LCSD05. October 2005.
* [Stroustrup14](#Stroustrup05) Stroustrup: [A Tour of C++](http://www.stroustrup.com/Tour.html).
  Addison Wesley 2014.
  Each chapter ends with an advice section consisting of a set of recommendations.
* [Stroustrup13](#Stroustrup13) Stroustrup: [The C++ Programming Language (4th Edition)](http://www.stroustrup.com/4th.html).
  Addison Wesley 2013.
  Each chapter ends with an advice section consisting of a set of recommendations.
* Stroustrup: [Style Guide](http://www.stroustrup.com/Programming/PPP-style.pdf)
  for [Programming: Principles and Practice using C++](http://www.stroustrup.com/programming.html).
  Mostly low-level naming and layout rules.
  Primarily a teaching tool.

## <a name="SS-Cplusplus"></a>RF.C++: C++ Programming (C++11/C++14)

* [TC++PL4](http://www.stroustrup.com/4th.html):
A thorough description of the C++ language and standard libraries for experienced programmers.
* [Tour++](http://www.stroustrup.com/Tour.html):
An overview of the C++ language and standard libraries for experienced programmers.
* [Programming: Principles and Practice using C++](http://www.stroustrup.com/programming.html):
A textbook for beginners and relative novices.

## <a name="SS-web"></a>RF.web: Websites

* [isocpp.org](https://isocpp.org)
* [Bjarne Stroustrup's home pages](http://www.stroustrup.com)
* [WG21](http://www.open-std.org/jtc1/sc22/wg21/)
* [Boost](http://www.boost.org)<a name="Boost"></a>
* [Adobe open source](http://www.adobe.com/open-source.html)
* [Poco libraries](http://pocoproject.org/)
* Sutter's Mill?
* ???

## <a name="SS-vid"></a>RS.video: Videos about "modern C++"

* Bjarne Stroustrup: [C++11 Style](http://channel9.msdn.com/Events/GoingNative/GoingNative-2012/Keynote-Bjarne-Stroustrup-Cpp11-Style). 2012.
* Bjarne Stroustrup: [The Essence of C++: With Examples in C++84, C++98, C++11, and C++14](http://channel9.msdn.com/Events/GoingNative/2013/Opening-Keynote-Bjarne-Stroustrup). 2013
* All the talks from [CppCon '14](https://isocpp.org/blog/2014/11/cppcon-videos-c9)
* Bjarne Stroustrup: [The essence of C++](https://www.youtube.com/watch?v=86xWVb4XIyE) at the University of Edinburgh. 2014.
* Bjarne Stroustrup: [The Evolution of C++ Past, Present and Future](https://www.youtube.com/watch?v=_wzc7a3McOs). CppCon 2016 keynote.
* Bjarne Stroustrup: [Make Simple Tasks Simple!](https://www.youtube.com/watch?v=nesCaocNjtQ). CppCon 2014 keynote.
* Bjarne Stroustrup: [Writing Good C++14](https://www.youtube.com/watch?v=1OEu9C51K2A). CppCon 2015 keynote about the Core Guidelines.
* Herb Sutter: [Writing Good C++14... By Default](https://www.youtube.com/watch?v=hEx5DNLWGgA). CppCon 2015 keynote about the Core Guidelines.
* CppCon 15
* ??? C++ Next
* ??? Meting C++
* ??? more ???

## <a name="SS-man"></a>RF.man: Manuals

* ISO C++ Standard C++11.
* ISO C++ Standard C++14.
* [ISO C++ Standard C++17](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4606.pdf). Committee Draft.
* [Palo Alto "Concepts" TR](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2012/n3351.pdf).
* [ISO C++ Concepts TS](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
* [WG21 Ranges report](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf). Draft.


## <a name="SS-core"></a>RF.core: Core Guidelines materials

This section contains materials that has been useful for presenting the core guidelines and the ideas behind them:

* [Our documents directory](https://github.com/isocpp/CppCoreGuidelines/tree/master/docs)
* Stroustrup, Sutter, and Dos Reis: [A brief introduction to C++'s model for type- and resource-safety](http://www.stroustrup.com/resource-model.pdf). A paper with lots of examples.
* Sergey Zubkov: [a Core Guidelines talk](https://www.youtube.com/watch?v=DyLwdl_6vmU)
and here are the [slides](http://2017.cppconf.ru/talks/sergey-zubkov). In Russian. 2017.
* Neil MacIntosh: [The Guideline Support Library: One Year Later](https://www.youtube.com/watch?v=_GhNnCuaEjo). CppCon 2016.
* Bjarne Stroustrup: [Writing Good C++14](https://www.youtube.com/watch?v=1OEu9C51K2A). CppCon 2015 keynote.
* Herb Sutter: [Writing Good C++14... By Default](https://www.youtube.com/watch?v=hEx5DNLWGgA). CppCon 2015 keynote.
* Peter Sommerlad: [C++ Core Guidelines - Modernize your C++ Code Base](https://www.youtube.com/watch?v=fQ926v4ZzAM). ACCU 2017.
* Bjarne Stroustrup: [No Littering!](https://www.youtube.com/watch?v=01zI9kV4h8c). Bay Area ACCU 2016.
It gives some idea of the ambition level for the Core Guidelines.

Note that slides for CppCon presentations are available (links with the posted videos).

Contributions to this list would be most welcome.

## <a name="SS-ack"></a>Acknowledgements

Thanks to the many people who contributed rules, suggestions, supporting information, references, etc.:

* Peter Juhl
* Neil MacIntosh
* Axel Naumann
* Andrew Pardoe
* Gabriel Dos Reis
* Zhuang, Jiangang (Jeff)
* Sergey Zubkov

and see the contributor list on the github.

# <a name="S-profile"></a>Pro: Profiles

Ideally, we would follow all of the guidelines.
That would give the cleanest, most regular, least error-prone, and often the fastest code.
Unfortunately, that is usually impossible because we have to fit our code into large code bases and use existing libraries.
Often, such code has been written over decades and does not follow these guidelines.
We must aim for [gradual adoption](#S-modernizing).

Whatever strategy for gradual adoption we adopt, we need to be able to apply sets of related guidelines to address some set
of problems first and leave the rest until later.
A similar idea of "related guidelines" becomes important when some, but not all, guidelines are considered relevant to a code base
or if a set of specialized guidelines is to be applied for a specialized application area.
We call such a set of related guidelines a "profile".
We aim for such a set of guidelines to be coherent so that they together help us reach a specific goal, such as "absence of range errors"
or "static type safety."
Each profile is designed to eliminate a class of errors.
Enforcement of "random" rules in isolation is more likely to be disruptive to a code base than delivering a definite improvement.

A "profile" is a set of deterministic and portably enforceable subset rules (i.e., restrictions) that are designed to achieve a specific guarantee.
"Deterministic" means they require only local analysis and could be implemented in a compiler (though they don't need to be).
"Portably enforceable" means they are like language rules, so programmers can count on different enforcement tools giving the same answer for the same code.

Code written to be warning-free using such a language profile is considered to conform to the profile.
Conforming code is considered to be safe by construction with regard to the safety properties targeted by that profile.
Conforming code will not be the root cause of errors for that property,
although such errors may be introduced into a program by other code, libraries or the external environment.
A profile may also introduce additional library types to ease conformance and encourage correct code.

Profiles summary:

* [Pro.type: Type safety](#SS-type)
* [Pro.bounds: Bounds safety](#SS-bounds)
* [Pro.lifetime: Lifetime safety](#SS-lifetime)

In the future, we expect to define many more profiles and add more checks to existing profiles.
Candidates include:

* narrowing arithmetic promotions/conversions (likely part of a separate safe-arithmetic profile)
* arithmetic cast from negative floating point to unsigned integral type (ditto)
* selected undefined behavior: Start with Gabriel Dos Reis's UB list developed for the WG21 study group
* selected unspecified behavior: Addressing portability concerns.
* `const` violations: Mostly done by compilers already, but we can catch inappropriate casting and underuse of `const`.

Enabling a profile is implementation defined; typically, it is set in the analysis tool used.

To suppress enforcement of a profile check, place a `suppress` annotation on a language contract. For example:

    [[suppress(bounds)]] char* raw_find(char* p, int n, char x)    // find x in p[0]..p[n - 1]
    {
        // ...
    }

Now `raw_find()` can scramble memory to its heart's content.
Obviously, suppression should be very rare.

## <a name="SS-type"></a>Pro.safety: Type-safety profile

This profile makes it easier to construct code that uses types correctly and avoids inadvertent type punning.
It does so by focusing on removing the primary sources of type violations, including unsafe uses of casts and unions.

For the purposes of this section,
type-safety is defined to be the property that a variable is not used in a way that doesn't obey the rules for the type of its definition.
Memory accessed as a type `T` should not be valid memory that actually contains an object of an unrelated type `U`.
Note that the safety is intended to be complete when combined also with [Bounds safety](#SS-bounds) and [Lifetime safety](#SS-lifetime).

An implementation of this profile shall recognize the following patterns in source code as non-conforming and issue a diagnostic.

Type safety profile summary:

* <a name="Pro-type-avoidcasts"></a>Type.1: [Avoid casts](#Res-casts):
<a name="Pro-type-reinterpretcast">a. </a>Don't use `reinterpret_cast`; A strict version of [Avoid casts](#Res-casts) and [prefer named casts](#Res-casts-named).
<a name="Pro-type-arithmeticcast">b. </a>Don't use `static_cast` for arithmetic types; A strict version of [Avoid casts](#Res-casts) and [prefer named casts](#Res-casts-named).
<a name="Pro-type-identitycast">c. </a>Don't cast between pointer types where the source type and the target type are the same; A strict version of [Avoid casts](#Res-casts).
<a name="Pro-type-implicitpointercast">d. </a>Don't cast between pointer types when the conversion could be implicit; A strict version of [Avoid casts](#Res-casts).
* <a name="Pro-type-downcast"></a>Type.2: Don't use `static_cast` to downcast:
[Use `dynamic_cast` instead](#Rh-dynamic_cast).
* <a name="Pro-type-constcast"></a>Type.3: Don't use `const_cast` to cast away `const` (i.e., at all):
[Don't cast away const](#Res-casts-const).
* <a name="Pro-type-cstylecast"></a>Type.4: Don't use C-style `(T)expression` or functional `T(expression)` casts:
Prefer [construction](#Res-construct) or [named casts](#Res-cast-named).
* <a name="Pro-type-init"></a>Type.5: Don't use a variable before it has been initialized:
[always initialize](#Res-always).
* <a name="Pro-type-memberinit"></a>Type.6: Always initialize a member variable:
[always initialize](#Res-always),
possibly using [default constructors](#Rc-default0) or
[default member initializers](#Rc-in-class-initializers).
* <a name="Pro-type-unon"></a>Type.7: Avoid naked union:
[Use `variant` instead](#Ru-naked).
* <a name="Pro-type-varargs"></a>Type.8: Avoid varargs:
[Don't use `va_arg` arguments](#F-varargs).

##### Impact

With the type-safety profile you can trust that every operation is applied to a valid object.
Exception may be thrown to indicate errors that cannot be detected statically (at compile time).
Note that this type-safety can be complete only if we also have [Bounds safety](#SS-bounds) and [Lifetime safety](#SS-lifetime).
Without those guarantees, a region of memory could be accessed independent of which object, objects, or parts of objects are stored in it.


## <a name="SS-bounds"></a>Pro.bounds: Bounds safety profile

This profile makes it easier to construct code that operates within the bounds of allocated blocks of memory.
It does so by focusing on removing the primary sources of bounds violations: pointer arithmetic and array indexing.
One of the core features of this profile is to restrict pointers to only refer to single objects, not arrays.

We define bounds-safety to be the property that a program does not use an object to access memory outside of the range that was allocated for it.
Bounds safety is intended to be complete only when combined with [Type safety](#SS-type) and [Lifetime safety](#SS-lifetime),
which cover other unsafe operations that allow bounds violations.

Bounds safety profile summary:

* <a href="Pro-bounds-arithmetic"></a>Bounds.1: Don't use pointer arithmetic. Use `span` instead:
[Pass pointers to single objects (only)](#Ri-array) and [Keep pointer arithmetic simple](#Res-ptr).
* <a href="Pro-bounds-arrayindex"></a>Bounds.2: Only index into arrays using constant expressions:
[Pass pointers to single objects (only)](#Ri-array) and [Keep pointer arithmetic simple](#Res-ptr).
* <a href="Pro-bounds-decay"></a>Bounds.3: No array-to-pointer decay:
[Pass pointers to single objects (only)](#Ri-array) and [Keep pointer arithmetic simple](#Res-ptr).
* <a href="Pro-bounds-stdlib"></a>Bounds.4: Don't use standard-library functions and types that are not bounds-checked:
[Use the standard library in a type-safe manner](#Rsl-bounds).

##### Impact

Bounds safety implies that access to an object - notably arrays - does not access beyond the object's memory allocation.
This eliminates a large class of insidious and hard-to-find errors, including the (in)famous "buffer overflow" errors.
This closes security loopholes as well as a prominent source of memory corruption (when writing out of bounds).
Even if an out-of-bounds access is "just a read", it can lead to invariant violations (when the accessed isn't of the assumed type)
and "mysterious values."


## <a name="SS-lifetime"></a>Pro.lifetime: Lifetime safety profile

Accessing through a pointer that doesn't point to anything is a major source of errors,
and very hard to avoid in many traditional C or C++ styles of programming.
For example, a pointer may be uninitialized, the `nullptr`, point beyond the range of an array, or to a deleted object.

[See the current design specification here.](https://github.com/isocpp/CppCoreGuidelines/blob/master/docs/Lifetime.pdf)

Lifetime safety profile summary:

* <a href="Pro-lifetime-invalid-deref"></a>Lifetime.1: Don't dereference a possibly invalid pointer:
[detect or avoid](#Res-deref).

##### Impact

Once completely enforced through a combination of style rules, static analysis, and library support, this profile

* eliminates one of the major sources of nasty errors in C++
* eliminates a major source of potential security violations
* improves performance by eliminating redundant "paranoia" checks
* increases confidence in correctness of code
* avoids undefined behavior by enforcing a key C++ language rule


# <a name="S-gsl"></a>GSL: Guidelines support library

The GSL is a small library of facilities designed to support this set of guidelines.
Without these facilities, the guidelines would have to be far more restrictive on language details.

The Core Guidelines support library is defined in namespace `gsl` and the names may be aliases for standard library or other well-known library names. Using the (compile-time) indirection through the `gsl` namespace allows for experimentation and for local variants of the support facilities.

The GSL is header only, and can be found at [GSL: Guidelines support library](https://github.com/Microsoft/GSL).
The support library facilities are designed to be extremely lightweight (zero-overhead) so that they impose no overhead compared to using conventional alternatives.
Where desirable, they can be "instrumented" with additional functionality (e.g., checks) for tasks such as debugging.

These Guidelines assume a `variant` type, but this is not currently in GSL.
Eventually, use [the one voted into C++17](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0088r3.html).

Summary of GSL components:

* [GSL.view: Views](#SS-views)
* [GSL.owner](#SS-ownership)
* [GSL.assert: Assertions](#SS-assertions)
* [GSL.util: Utilities](#SS-utilities)
* [GSL.concept: Concepts](#SS-gsl-concepts)

We plan for a "ISO C++ standard style" semi-formal specification of the GSL.

We rely on the ISO C++ Standard Library and hope for parts of the GSL to be absorbed into the standard library.

## <a name="SS-views"></a>GSL.view: Views

These types allow the user to distinguish between owning and non-owning pointers and between pointers to a single object and pointers to the first element of a sequence.

These "views" are never owners.

References are never owners (see [R.4](#Rr-ref). Note: References have many opportunities to outlive the objects they refer to (returning a local variable by reference, holding a reference to an element of a vector and doing `push_back`, binding to `std::max(x, y + 1)`, etc. The Lifetime safety profile aims to address those things, but even so `owner<T&>` does not make sense and is discouraged.

The names are mostly ISO standard-library style (lower case and underscore):

* `T*`      // The `T*` is not an owner, may be null; assumed to be pointing to a single element.
* `T&`      // The `T&` is not an owner and can never be a "null reference"; references are always bound to objects.

The "raw-pointer" notation (e.g. `int*`) is assumed to have its most common meaning; that is, a pointer points to an object, but does not own it.
Owners should be converted to resource handles (e.g., `unique_ptr` or `vector<T>`) or marked `owner<T*>`.

* `owner<T*>`   // a `T*` that owns the object pointed/referred to; may be `nullptr`.

`owner` is used to mark owning pointers in code that cannot be upgraded to use proper resource handles.
Reasons for that include:

* Cost of conversion.
* The pointer is used with an ABI.
* The pointer is part of the implementation of a resource handle.

An `owner<T>` differs from a resource handle for a `T` by still requiring an explicit `delete`.

An `owner<T>` is assumed to refer to an object on the free store (heap).

If something is not supposed to be `nullptr`, say so:

* `not_null<T>`   // `T` is usually a pointer type (e.g., `not_null<int*>` and `not_null<owner<Foo*>>`) that may not be `nullptr`.
  `T` can be any type for which `==nullptr` is meaningful.

* `span<T>`       // `[p:p+n)`, constructor from `{p, q}` and `{p, n}`; `T` is the pointer type
* `span_p<T>`     // `{p, predicate}` `[p:q)` where `q` is the first element for which `predicate(*p)` is true
* `string_span`   // `span<char>`
* `cstring_span`  // `span<const char>`

A `span<T>` refers to zero or more mutable `T`s unless `T` is a `const` type.

"Pointer arithmetic" is best done within `span`s.
A `char*` that points to more than one `char` but is not a C-style string (e.g., a pointer into an input buffer) should be represented by a `span`.

* `zstring`    // a `char*` supposed to be a C-style string; that is, a zero-terminated sequence of `char` or `nullptr`
* `czstring`   // a `const char*` supposed to be a C-style string; that is, a zero-terminated sequence of `const` `char` or `nullptr`

Logically, those last two aliases are not needed, but we are not always logical, and they make the distinction between a pointer to one `char` and a pointer to a C-style string explicit.
A sequence of characters that is not assumed to be zero-terminated should be a `char*`, rather than a `zstring`.
French accent optional.

Use `not_null<zstring>` for C-style strings that cannot be `nullptr`. ??? Do we need a name for `not_null<zstring>`? or is its ugliness a feature?

## <a name="SS-ownership"></a>GSL.owner: Ownership pointers

* `unique_ptr<T>`     // unique ownership: `std::unique_ptr<T>`
* `shared_ptr<T>`     // shared ownership: `std::shared_ptr<T>` (a counted pointer)
* `stack_array<T>`    // A stack-allocated array. The number of elements are determined at construction and fixed thereafter. The elements are mutable unless `T` is a `const` type.
* `dyn_array<T>`      // ??? needed ??? A heap-allocated array. The number of elements are determined at construction and fixed thereafter.
  The elements are mutable unless `T` is a `const` type. Basically a `span` that allocates and owns its elements.

## <a name="SS-assertions"></a>GSL.assert: Assertions

* `Expects`     // precondition assertion. Currently placed in function bodies. Later, should be moved to declarations.
                // `Expects(p)` terminates the program unless `p == true`
                // `Expect` in under control of some options (enforcement, error message, alternatives to terminate)
* `Ensures`     // postcondition assertion. Currently placed in function bodies. Later, should be moved to declarations.

These assertions are currently macros (yuck!) and must appear in function definitions (only)
pending standard committee decisions on contracts and assertion syntax.
See [the contract proposal](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0380r1.pdf); using the attribute syntax,
for example, `Expects(p)` will become `[[expects: p]]`.

## <a name="SS-utilities"></a>GSL.util: Utilities

* `finally`        // `finally(f)` makes a `final_action{f}` with a destructor that invokes `f`
* `narrow_cast`    // `narrow_cast<T>(x)` is `static_cast<T>(x)`
* `narrow`         // `narrow<T>(x)` is `static_cast<T>(x)` if `static_cast<T>(x) == x` or it throws `narrowing_error`
* `[[implicit]]`   // "Marker" to put on single-argument constructors to explicitly make them non-explicit.
* `move_owner`     // `p = move_owner(q)` means `p = q` but ???
* `joining_thread` // a RAII style version of `std::thread` that joins.
* `index`          // a type to use for all container and array indexing (currently an alias for `ptrdiff_t`)

## <a name="SS-gsl-concepts"></a>GSL.concept: Concepts

These concepts (type predicates) are borrowed from
Andrew Sutton's Origin library,
the Range proposal,
and the ISO WG21 Palo Alto TR.
They are likely to be very similar to what will become part of the ISO C++ standard.
The notation is that of the ISO WG21 [Concepts TS](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4553.pdf).
Most of the concepts below are defined in [the Ranges TS](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/n4569.pdf).

* `Range`
* `String`   // ???
* `Number`   // ???
* `Sortable`
* `Pointer`  // A type with `*`, `->`, `==`, and default construction (default construction is assumed to set the singular "null" value); see [smart pointers](#SS-gsl-smartptrconcepts)
* `Unique_ptr`  // A type that matches `Pointer`, has move (not copy), and matches the Lifetime profile criteria for a `unique` owner type; see [smart pointers](#SS-gsl-smartptrconcepts)
* `Shared_ptr`   // A type that matches `Pointer`, has copy, and matches the Lifetime profile criteria for a `shared` owner type; see [smart pointers](#SS-gsl-smartptrconcepts)
* `EqualityComparable`   // ???Must we suffer CaMelcAse???
* `Convertible`
* `Common`
* `Boolean`
* `Integral`
* `SignedIntegral`
* `SemiRegular` // ??? Copyable?
* `Regular`
* `TotallyOrdered`
* `Function`
* `RegularFunction`
* `Predicate`
* `Relation`
* ...

### <a name="SS-gsl-smartptrconcepts"></a>GSL.ptr: Smart pointer concepts

See [Lifetime paper](https://github.com/isocpp/CppCoreGuidelines/blob/master/docs/Lifetime.pdf).

# <a name="S-naming"></a>NL: Naming and layout rules

Consistent naming and layout are helpful.
If for no other reason because it minimizes "my style is better than your style" arguments.
However, there are many, many, different styles around and people are passionate about them (pro and con).
Also, most real-world projects includes code from many sources, so standardizing on a single style for all code is often impossible.
After many requests for guidance from users, we present a set of rules that you might use if you have no better ideas, but the real aim is consistency, rather than any particular rule set.
IDEs and tools can help (as well as hinder).

Naming and layout rules:

* [NL.1: Don't say in comments what can be clearly stated in code](#Rl-comments)
* [NL.2: State intent in comments](#Rl-comments-intent)
* [NL.3: Keep comments crisp](#Rl-comments-crisp)
* [NL.4: Maintain a consistent indentation style](#Rl-indent)
* [NL.5: Avoid encoding type information in names](#Rl-name-type)
* [NL.7: Make the length of a name roughly proportional to the length of its scope](#Rl-name-length)
* [NL.8: Use a consistent naming style](#Rl-name)
* [NL.9: Use `ALL_CAPS` for macro names only](#Rl-all-caps)
* [NL.10: Prefer `underscore_style` names](#Rl-camel)
* [NL.11: Make literals readable](#Rl-literals)
* [NL.15: Use spaces sparingly](#Rl-space)
* [NL.16: 使用约定的类成员声明顺序](#Rl-order)
* [NL.17: Use K&R-derived layout](#Rl-knr)
* [NL.18: Use C++-style declarator layout](#Rl-ptr)
* [NL.19: Avoid names that are easily misread](#Rl-misread)
* [NL.20: Don't place two statements on the same line](#Rl-stmt)
* [NL.21: Declare one name (only) per declaration](#Rl-dcl)
* [NL.25: Don't use `void` as an argument type](#Rl-void)
* [NL.26: Use conventional `const` notation](#Rl-const)

Most of these rules are aesthetic and programmers hold strong opinions.
IDEs also tend to have defaults and a range of alternatives.
These rules are suggested defaults to follow unless you have reasons not to.

We have had comments to the effect that naming and layout are so personal and/or arbitrary that we should not try to "legislate" them.
We are not "legislating" (see the previous paragraph).
However, we have had many requests for a set of naming and layout conventions to use when there are no external constraints.

More specific and detailed rules are easier to enforce.

These rules bear a strong resemblance to the recommendations in the [PPP Style Guide](http://www.stroustrup.com/Programming/PPP-style.pdf)
written in support of Stroustrup's [Programming: Principles and Practice using C++](http://www.stroustrup.com/programming.html).

### <a name="Rl-comments"></a>NL.1: Don't say in comments what can be clearly stated in code

##### Reason

Compilers do not read comments.
Comments are less precise than code.
Comments are not updated as consistently as code.

##### Example, bad

    auto x = m * v1 + vv;   // multiply m with v1 and add the result to vv

##### Enforcement

Build an AI program that interprets colloquial English text and see if what is said could be better expressed in C++.

### <a name="Rl-comments-intent"></a>NL.2: State intent in comments

##### Reason

Code says what is done, not what is supposed to be done. Often intent can be stated more clearly and concisely than the implementation.

##### Example

    void stable_sort(Sortable& c)
        // sort c in the order determined by <, keep equal elements (as defined by ==) in
        // their original relative order
    {
        // ... quite a few lines of non-trivial code ...
    }

##### Note

If the comment and the code disagree, both are likely to be wrong.

### <a name="Rl-comments-crisp"></a>NL.3: Keep comments crisp

##### Reason

Verbosity slows down understanding and makes the code harder to read by spreading it around in the source file.

##### Note

Use intelligible English.
I may be fluent in Danish, but most programmers are not; the maintainers of my code may not be.
Avoid SMS lingo and watch your grammar, punctuation, and capitalization.
Aim for professionalism, not "cool."

##### Enforcement

not possible.

### <a name="Rl-indent"></a>NL.4: Maintain a consistent indentation style

##### Reason

Readability. Avoidance of "silly mistakes."

##### Example, bad

    int i;
    for (i = 0; i < max; ++i); // bug waiting to happen
    if (i == j)
        return i;

##### Note

Always indenting the statement after `if (...)`, `for (...)`, and `while (...)` is usually a good idea:

    if (i < 0) error("negative argument");

    if (i < 0)
        error("negative argument");

##### Enforcement

Use a tool.

### <a name="Rl-name-type"></a>NL.5: Avoid encoding type information in names

##### Rationale

If names reflect types rather than functionality, it becomes hard to change the types used to provide that functionality.
Also, if the type of a variable is changed, code using it will have to be modified.
Minimize unintentional conversions.

##### Example, bad

    void print_int(int i);
    void print_string(const char*);

    print_int(1);          // repetitive, manual type matching
    print_string("xyzzy"); // repetitive, manual type matching

##### Example, good

    void print(int i);
    void print(string_view);    // also works on any string-like sequence

    print(1);              // clear, automatic type matching
    print("xyzzy");        // clear, automatic type matching

##### Note

Names with types encoded are either verbose or cryptic.

    printS  // print a std::string
    prints  // print a C-style string
    printi  // print an int

Requiring techniques like Hungarian notation to encode a type has been used in untyped languages, but is generally unnecessary and actively harmful in a strongly statically-typed language like C++, because the annotations get out of date (the warts are just like comments and rot just like them) and they interfere with good use of the language (use the same name and overload resolution instead).

##### Note

Some styles use very general (not type-specific) prefixes to denote the general use of a variable.

    auto p = new User();
    auto p = make_unique<User>();
    // note: "p" is not being used to say "raw pointer to type User,"
    //       just generally to say "this is an indirection"

    auto cntHits = calc_total_of_hits(/*...*/);
    // note: "cnt" is not being used to encode a type,
    //       just generally to say "this is a count of something"

This is not harmful and does not fall under this guideline because it does not encode type information.

##### Note

Some styles distinguish members from local variable, and/or from global variable.

    struct S {
        int m_;
        S(int m) :m_{abs(m)} { }
    };

This is not harmful and does not fall under this guideline because it does not encode type information.

##### Note

Like C++, some styles distinguish types from non-types.
For example, by capitalizing type names, but not the names of functions and variables.

    typename<typename T>
    class HashTable {   // maps string to T
        // ...
    };

    HashTable<int> index;

This is not harmful and does not fall under this guideline because it does not encode type information.

### <a name="Rl-name-length"></a>NL.7: Make the length of a name roughly proportional to the length of its scope

**Rationale**: The larger the scope the greater the chance of confusion and of an unintended name clash.

##### Example

    double sqrt(double x);   // return the square root of x; x must be non-negative

    int length(const char* p);  // return the number of characters in a zero-terminated C-style string

    int length_of_string(const char zero_terminated_array_of_char[])    // bad: verbose

    int g;      // bad: global variable with a cryptic name

    int open;   // bad: global variable with a short, popular name

The use of `p` for pointer and `x` for a floating-point variable is conventional and non-confusing in a restricted scope.

##### Enforcement

???

### <a name="Rl-name"></a>NL.8: Use a consistent naming style

**Rationale**: Consistence in naming and naming style increases readability.

##### Note

There are many styles and when you use multiple libraries, you can't follow all their different conventions.
Choose a "house style", but leave "imported" libraries with their original style.

##### Example

ISO Standard, use lower case only and digits, separate words with underscores:

* `int`
* `vector`
* `my_map`

Avoid double underscores `__`.

##### Example

[Stroustrup](http://www.stroustrup.com/Programming/PPP-style.pdf):
ISO Standard, but with upper case used for your own types and concepts:

* `int`
* `vector`
* `My_map`

##### Example

CamelCase: capitalize each word in a multi-word identifier:

* `int`
* `vector`
* `MyMap`
* `myMap`

Some conventions capitalize the first letter, some don't.

##### Note

Try to be consistent in your use of acronyms and lengths of identifiers:

    int mtbf {12};
    int mean_time_between_failures {12}; // make up your mind

##### Enforcement

Would be possible except for the use of libraries with varying conventions.

### <a name="Rl-all-caps"></a>NL.9: Use `ALL_CAPS` for macro names only

##### Reason

To avoid confusing macros with names that obey scope and type rules.

##### Example

    void f()
    {
        const int SIZE{1000};  // Bad, use 'size' instead
        int v[SIZE];
    }

##### Note

This rule applies to non-macro symbolic constants:

    enum bad { BAD, WORSE, HORRIBLE }; // BAD

##### Enforcement

* Flag macros with lower-case letters
* Flag `ALL_CAPS` non-macro names

### <a name="Rl-camel"></a>NL.10: Prefer `underscore_style` names

##### Reason

The use of underscores to separate parts of a name is the original C and C++ style and used in the C++ Standard Library.

##### Note

This rule is a default to use only if you have a choice.
Often, you don't have a choice and must follow an established style for [consistency](#Rl-name).
The need for consistency beats personal taste.

This is a recommendation for [when you have no constraints or better ideas](#S-naming).
Thus rule was added after many requests for guidance.

##### Example

[Stroustrup](http://www.stroustrup.com/Programming/PPP-style.pdf):
ISO Standard, but with upper case used for your own types and concepts:

* `int`
* `vector`
* `My_map`

##### Enforcement

Impossible.

### <a name="Rl-space"></a>NL.15: Use spaces sparingly

##### Reason

Too much space makes the text larger and distracts.

##### Example, bad

    #include < map >

    int main(int argc, char * argv [ ])
    {
        // ...
    }

##### Example

    #include <map>

    int main(int argc, char* argv[])
    {
        // ...
    }

##### Note

Some IDEs have their own opinions and add distracting space.

This is a recommendation for [when you have no constraints or better ideas](#S-naming).
Thus rule was added after many requests for guidance.

##### Note

We value well-placed whitespace as a significant help for readability. Just don't overdo it.

### <a name="Rl-literals"></a>NL.11: Make literals readable

##### Reason

Readability.

##### Example

Use digit separators to avoid long strings of digits

    auto c = 299'792'458; // m/s2
    auto q2 = 0b0000'1111'0000'0000;
    auto ss_number = 123'456'7890;

##### Example

Use literal suffixes where clarification is needed

    auto hello = "Hello!"s; // a std::string
    auto world = "world";   // a C-style string
    auto interval = 100ms;  // using <chrono>

##### Note

Literals should not be sprinkled all over the code as ["magic constants"](#Res-magic),
but it is still a good idea to make them readable where they are defined.
It is easy to make a typo in a long string of integers.

##### Enforcement

Flag long digit sequences. The trouble is to define "long"; maybe 7.

### <a name="Rl-order"></a>NL.16: 使用约定的类成员声明顺序

##### Reason

A conventional order of members improves readability.

When declaring a class use the following order

* types: classes, enums, and aliases (`using`)
* constructors, assignments, destructor
* functions
* data

Use the `public` before `protected` before `private` order.

This is a recommendation for [when you have no constraints or better ideas](#S-naming).
Thus rule was added after many requests for guidance.

##### Example

    class X {
    public:
        // interface
    protected:
        // unchecked function for use by derived class implementations
    private:
        // implementation details
    };

##### Example

Sometimes, the default order of members conflicts with a desire to separate the public interface from implementation details.
In such cases, private types and functions can be placed with private data.

    class X {
    public:
        // interface
    protected:
        // unchecked function for use by derived class implementations
    private:
        // implementation details (types, functions, and data)
    };

##### Example, bad

Avoid multiple blocks of declarations of one access (e.g., `public`) dispersed among blocks of declarations with different access (e.g. `private`).

    class X {   // bad
    public:
        void f();
    public:
        int g();
        // ...
    };

The use of macros to declare groups of members often leads to violation of any ordering rules.
However, macros obscures what is being expressed anyway.

##### Enforcement

Flag departures from the suggested order. There will be a lot of old code that doesn't follow this rule.

### <a name="Rl-knr"></a>NL.17: Use K&R-derived layout

##### Reason

This is the original C and C++ layout. It preserves vertical space well. It distinguishes different language constructs (such as functions and classes) well.

##### Note

In the context of C++, this style is often called "Stroustrup".

This is a recommendation for [when you have no constraints or better ideas](#S-naming).
Thus rule was added after many requests for guidance.

##### Example

    struct Cable {
        int x;
        // ...
    };

    double foo(int x)
    {
        if (0 < x) {
            // ...
        }

        switch (x) {
        case 0:
            // ...
            break;
        case amazing:
            // ...
            break;
        default:
            // ...
            break;
        }

        if (0 < x)
            ++x;

        if (x < 0)
            something();
        else
            something_else();

        return some_value;
    }

Note the space between `if` and `(`

##### Note

Use separate lines for each statement, the branches of an `if`, and the body of a `for`.

##### Note

The `{` for a `class` and a `struct` is *not* on a separate line, but the `{` for a function is.

##### Note

Capitalize the names of your user-defined types to distinguish them from standards-library types.

##### Note

Do not capitalize function names.

##### Enforcement

If you want enforcement, use an IDE to reformat.

### <a name="Rl-ptr"></a>NL.18: Use C++-style declarator layout

##### Reason

The C-style layout emphasizes use in expressions and grammar, whereas the C++-style emphasizes types.
The use in expressions argument doesn't hold for references.

##### Example

    T& operator[](size_t);   // OK
    T &operator[](size_t);   // just strange
    T & operator[](size_t);   // undecided

##### Note

This is a recommendation for [when you have no constraints or better ideas](#S-naming).
Thus rule was added after many requests for guidance.

##### Enforcement

Impossible in the face of history.


### <a name="Rl-misread"></a>NL.19: Avoid names that are easily misread

##### Reason

Readability.
Not everyone has screens and printers that make it easy to distinguish all characters.
We easily confuse similarly spelled and slightly misspelled words.

##### Example

    int oO01lL = 6; // bad

    int splunk = 7;
    int splonk = 8; // bad: splunk and splonk are easily confused

##### Enforcement

???

### <a name="Rl-stmt"></a>NL.20: Don't place two statements on the same line

##### Reason

Readability.
It is really easy to overlook a statement when there is more on a line.

##### Example

    int x = 7; char* p = 29;    // don't
    int x = 7; f(x);  ++x;      // don't

##### Enforcement

Easy.

### <a name="Rl-dcl"></a>NL.21: Declare one name (only) per declaration

##### Reason

Readability.
Minimizing confusion with the declarator syntax.

##### Note

For details, see [ES.10](#Res-name-one).


### <a name="Rl-void"></a>NL.25: Don't use `void` as an argument type

##### Reason

It's verbose and only needed where C compatibility matters.

##### Example

    void f(void);   // bad

    void g();       // better

##### Note

Even Dennis Ritchie deemed `void f(void)` an abomination.
You can make an argument for that abomination in C when function prototypes were rare so that banning:

    int f();
    f(1, 2, "weird but valid C89");   // hope that f() is defined int f(a, b, c) char* c; { /* ... */ }

would have caused major problems, but not in the 21st century and in C++.

### <a name="Rl-const"></a>NL.26: Use conventional `const` notation

##### Reason

Conventional notation is more familiar to more programmers.
Consistency in large code bases.

##### Example

    const int x = 7;    // OK
    int const y = 9;    // bad

    const int *const p = nullptr;   // OK, constant pointer to constant int
    int const *const p = nullptr;   // bad, constant pointer to constant int

##### Note

We are well aware that you could claim the "bad" examples more logical than the ones marked "OK",
but they also confuse more people, especially novices relying on teaching material using the far more common, conventional OK style.

As ever, remember that the aim of these naming and layout rules is consistency and that aesthetics vary immensely.

This is a recommendation for [when you have no constraints or better ideas](#S-naming).
Thus rule was added after many requests for guidance.

##### Enforcement

Flag `const` used as a suffix for a type.

# <a name="S-faq"></a>FAQ: Answers to frequently asked questions

This section covers answers to frequently asked questions about these guidelines.

### <a name="Faq-aims"></a>FAQ.1: What do these guidelines aim to achieve?

See the <a href="#S-abstract">top of this page</a>. This is an open-source project to maintain modern authoritative guidelines for writing C++ code using the current C++ Standard (as of this writing, C++14). The guidelines are designed to be modern, machine-enforceable wherever possible, and open to contributions and forking so that organizations can easily incorporate them into their own corporate coding guidelines.

### <a name="Faq-announced"></a>FAQ.2: When and where was this work first announced?

It was announced by [Bjarne Stroustrup in his CppCon 2015 opening keynote, "Writing Good C++14"](https://isocpp.org/blog/2015/09/stroustrup-cppcon15-keynote). See also the [accompanying isocpp.org blog post](https://isocpp.org/blog/2015/09/bjarne-stroustrup-announces-cpp-core-guidelines), and for the rationale of the type and memory safety guidelines see [Herb Sutter's follow-up CppCon 2015 talk, "Writing Good C++14 ... By Default"](https://isocpp.org/blog/2015/09/sutter-cppcon15-day2plenary).

### <a name="Faq-maintainers"></a>FAQ.3: Who are the authors and maintainers of these guidelines?

The initial primary authors and maintainers are Bjarne Stroustrup and Herb Sutter, and the guidelines so far were developed with contributions from experts at CERN, Microsoft, Morgan Stanley, and several other organizations. At the time of their release, the guidelines are in a "0.6" state, and contributions are welcome. As Stroustrup said in his announcement: "We need help!"

### <a name="Faq-contribute"></a>FAQ.4: How can I contribute?

See [CONTRIBUTING.md](https://github.com/isocpp/CppCoreGuidelines/blob/master/CONTRIBUTING.md). We appreciate volunteer help!

### <a name="Faq-maintainer"></a>FAQ.5: How can I become an editor/maintainer?

By contributing a lot first and having the consistent quality of your contributions recognized. See [CONTRIBUTING.md](https://github.com/isocpp/CppCoreGuidelines/blob/master/CONTRIBUTING.md). We appreciate volunteer help!

### <a name="Faq-iso"></a>FAQ.6: Have these guidelines been approved by the ISO C++ standards committee? Do they represent the consensus of the committee?

No. These guidelines are outside the standard. They are intended to serve the standard, and be maintained as current guidelines about how to use the current Standard C++ effectively. We aim to keep them in sync with the standard as that is evolved by the committee.

### <a name="Faq-isocpp"></a>FAQ.7: If these guidelines are not approved by the committee, why are they under `github.com/isocpp`?

Because `isocpp` is the Standard C++ Foundation; the committee's repositories are under [github.com/*cplusplus*](https://github.com/cplusplus). Some neutral organization has to own the copyright and license to make it clear this is not being dominated by any one person or vendor. The natural entity is the Foundation, which exists to promote the use and up-to-date understanding of modern Standard C++ and the work of the committee. This follows the same pattern that isocpp.org did for the [C++ FAQ](https://isocpp.org/faq), which was initially the work of Bjarne Stroustrup, Marshall Cline, and Herb Sutter and contributed to the open project in the same way.

### <a name="Faq-cpp98"></a>FAQ.8: Will there be a C++98 version of these Guidelines? a C++11 version?

No. These guidelines are about how to best use Standard C++14 (and, if you have an implementation available, the Concepts Technical Specification) and write code assuming you have a modern conforming compiler.

### <a name="Faq-language-extensions"></a>FAQ.9: Do these guidelines propose new language features?

No. These guidelines are about how to best use Standard C++14 + the Concepts Technical Specification, and they limit themselves to recommending only those features.

### <a name="Faq-markdown"></a>FAQ.10: What version of Markdown do these guidelines use?

These coding standards are written using [CommonMark](http://commonmark.org), and `<a>` HTML anchors.

We are considering the following extensions from [GitHub Flavored Markdown (GFM)](https://help.github.com/articles/github-flavored-markdown/):

* fenced code blocks (consistently using indented vs. fenced is under discussion)
* tables (none yet but we'll likely need them, and this is a GFM extension)

Avoid other HTML tags and other extensions.

Note: We are not yet consistent with this style.

### <a name="Faq-gsl"></a>FAQ.50: What is the GSL (guidelines support library)?

The GSL is the small set of types and aliases specified in these guidelines. As of this writing, their specification herein is too sparse; we plan to add a WG21-style interface specification to ensure that different implementations agree, and to propose as a contribution for possible standardization, subject as usual to whatever the committee decides to accept/improve/alter/reject.

### <a name="Faq-msgsl"></a>FAQ.51: Is [github.com/Microsoft/GSL](https://github.com/Microsoft/GSL) the GSL?

No. That is just a first implementation contributed by Microsoft. Other implementations by other vendors are encouraged, as are forks of and contributions to that implementation. As of this writing one week into the public project, at least one GPLv3 open-source implementation already exists. We plan to produce a WG21-style interface specification to ensure that different implementations agree.

### <a name="Faq-gsl-implementation"></a>FAQ.52: Why not supply an actual GSL implementation in/with these guidelines?

We are reluctant to bless one particular implementation because we do not want to make people think there is only one, and inadvertently stifle parallel implementations. And if these guidelines included an actual implementation, then whoever contributed it could be mistakenly seen as too influential. We prefer to follow the long-standing approach of the committee, namely to specify interfaces, not implementations. But at the same time we want at least one implementation available; we hope for many.

### <a name="Faq-boost"></a>FAQ.53: Why weren't the GSL types proposed through Boost?

Because we want to use them immediately, and because they are temporary in that we want to retire them as soon as types that fill the same needs exist in the standard library.

### <a name="Faq-gsl-iso"></a>FAQ.54: Has the GSL (guidelines support library) been approved by the ISO C++ standards committee?

No. The GSL exists only to supply a few types and aliases that are not currently in the standard library. If the committee decides on standardized versions (of these or other types that fill the same need) then they can be removed from the GSL.

### <a name="Faq-gsl-string-view"></a>FAQ.55: If you're using the standard types where available, why is the GSL `string_span` different from the `string_view` in the Library Fundamentals 1 Technical Specification and C++17 Working Paper? Why not just use the committee-approved `string_view`?

The consensus on the taxonomy of views for the C++ Standard Library was that "view" means "read-only", and "span" means "read/write". The read-only `string_view` was the first such component to complete the standardization process, while `span` and `string_span` are currently being considered for standardization.

### <a name="Faq-gsl-owner"></a>FAQ.56: Is `owner` the same as the proposed `observer_ptr`?

No. `owner` owns, is an alias, and can be applied to any indirection type. The main intent of `observer_ptr` is to signify a *non*-owning pointer.

### <a name="Faq-gsl-stack-array"></a>FAQ.57: Is `stack_array` the same as the standard `array`?

No. `stack_array` is guaranteed to be allocated on the stack. Although a `std::array` contains its storage directly inside itself, the `array` object can be put anywhere, including the heap.

### <a name="Faq-gsl-dyn-array"></a>FAQ.58: Is `dyn_array` the same as `vector` or the proposed `dynarray`?

No. `dyn_array` is not resizable, and is a safe way to refer to a heap-allocated fixed-size array. Unlike `vector`, it is intended to replace array-`new[]`. Unlike the `dynarray` that has been proposed in the committee, this does not anticipate compiler/language magic to somehow allocate it on the stack when it is a member of an object that is allocated on the stack; it simply refers to a "dynamic" or heap-based array.

### <a name="Faq-gsl-expects"></a>FAQ.59: Is `Expects` the same as `assert`?

No. It is a placeholder for language support for contract preconditions.

### <a name="Faq-gsl-ensures"></a>FAQ.60: Is `Ensures` the same as `assert`?

No. It is a placeholder for language support for contract postconditions.

# <a name="S-libraries"></a>Appendix A: Libraries

This section lists recommended libraries, and explicitly recommends a few.

??? Suitable for the general guide? I think not ???

# <a name="S-modernizing"></a>Appendix B: Modernizing code

Ideally, we follow all rules in all code.
Realistically, we have to deal with a lot of old code:

* application code written before the guidelines were formulated or known
* libraries written to older/different standards
* code written under "unusual" constraints
* code that we just haven't gotten around to modernizing

If we have a million lines of new code, the idea of "just changing it all at once" is typically unrealistic.
Thus, we need a way of gradually modernizing a code base.

Upgrading older code to modern style can be a daunting task.
Often, the old code is both a mess (hard to understand) and working correctly (for the current range of uses).
Typically, the original programmer is not around and the test cases incomplete.
The fact that the code is a mess dramatically increases the effort needed to make any change and the risk of introducing errors.
Often, messy old code runs unnecessarily slowly because it requires outdated compilers and cannot take advantage of modern hardware.
In many cases, automated "modernizer"-style tool support would be required for major upgrade efforts.

The purpose of modernizing code is to simplify adding new functionality, to ease maintenance, and to increase performance (throughput or latency), and to better utilize modern hardware.
Making code "look pretty" or "follow modern style" are not by themselves reasons for change.
There are risks implied by every change and costs (including the cost of lost opportunities) implied by having an outdated code base.
The cost reductions must outweigh the risks.

But how?

There is no one approach to modernizing code.
How best to do it depends on the code, the pressure for updates, the backgrounds of the developers, and the available tool.
Here are some (very general) ideas:

* The ideal is "just upgrade everything." That gives the most benefits for the shortest total time.
  In most circumstances, it is also impossible.
* We could convert a code base module for module, but any rules that affects interfaces (especially ABIs), such as [use `span`](#SS-views), cannot be done on a per-module basis.
* We could convert code "bottom up" starting with the rules we estimate will give the greatest benefits and/or the least trouble in a given code base.
* We could start by focusing on the interfaces, e.g., make sure that no resources are lost and no pointer is misused.
  This would be a set of changes across the whole code base, but would most likely have huge benefits.
  Afterwards, code hidden behind those interfaces can be gradually modernized without affecting other code.

Whichever way you choose, please note that the most advantages come with the highest conformance to the guidelines.
The guidelines are not a random set of unrelated rules where you can randomly pick and choose with an expectation of success.

We would dearly love to hear about experience and about tools used.
Modernization can be much faster, simpler, and safer when supported with analysis tools and even code transformation tools.

# <a name="S-discussion"></a>Appendix C: Discussion

This section contains follow-up material on rules and sets of rules.
In particular, here we present further rationale, longer examples, and discussions of alternatives.

### <a name="Sd-order"></a>Discussion: Define and initialize member variables in the order of member declaration

Member variables are always initialized in the order they are declared in the class definition, so write them in that order in the constructor initialization list. Writing them in a different order just makes the code confusing because it won't run in the order you see, and that can make it hard to see order-dependent bugs.

    class Employee {
        string email, first, last;
    public:
        Employee(const char* firstName, const char* lastName);
        // ...
    };

    Employee::Employee(const char* firstName, const char* lastName)
      : first(firstName),
        last(lastName),
        // BAD: first and last not yet constructed
        email(first + "." + last + "@acme.com")
    {}

In this example, `email` will be constructed before `first` and `last` because it is declared first. That means its constructor will attempt to use `first` and `last` too soon -- not just before they are set to the desired values, but before they are constructed at all.

If the class definition and the constructor body are in separate files, the long-distance influence that the order of member variable declarations has over the constructor's correctness will be even harder to spot.

**References**:

[\[Cline99\]](#Cline99) §22.03-11, [\[Dewhurst03\]](#Dewhurst03) §52-53, [\[Koenig97\]](#Koenig97) §4, [\[Lakos96\]](#Lakos96) §10.3.5, [\[Meyers97\]](#Meyers97) §13, [\[Murray93\]](#Murray93) §2.1.3, [\[Sutter00\]](#Sutter00) §47

### <a name="Sd-init"></a>Discussion: Use of `=`, `{}`, and `()` as initializers

???

### <a name="Sd-factory"></a>Discussion: Use a factory function if you need "virtual behavior" during initialization

If your design wants virtual dispatch into a derived class from a base class constructor or destructor for functions like `f` and `g`, you need other techniques, such as a post-constructor -- a separate member function the caller must invoke to complete initialization, which can safely call `f` and `g` because in member functions virtual calls behave normally. Some techniques for this are shown in the References. Here's a non-exhaustive list of options:

* *Pass the buck:* Just document that user code must call the post-initialization function right after constructing an object.
* *Post-initialize lazily:* Do it during the first call of a member function. A Boolean flag in the base class tells whether or not post-construction has taken place yet.
* *Use virtual base class semantics:* Language rules dictate that the constructor most-derived class decides which base constructor will be invoked; you can use that to your advantage. (See [\[Taligent94\]](#Taligent94).)
* *Use a factory function:* This way, you can easily force a mandatory invocation of a post-constructor function.

Here is an example of the last option:

    class B {
    public:
        B() { /* ... */ f(); /* ... */ }   // BAD: see Item 49.1

        virtual void f() = 0;

        // ...
    };

    class B {
    protected:
        B() { /* ... */ }
        virtual void post_initialize()    // called right after construction
            { /* ... */ f(); /* ... */ }   // GOOD: virtual dispatch is safe
    public:
        virtual void f() = 0;

        template<class T>
        static shared_ptr<T> create()    // interface for creating objects
        {
            auto p = make_shared<T>();
            p->post_initialize();
            return p;
        }
    };


    class D : public B {                 // some derived class
    public:
        void f() override { /* ...  */ };

    protected:
        D() {}

        template<class T>
        friend shared_ptr<T> B::Create();
    };

    shared_ptr<D> p = D::Create<D>();    // creating a D object

This design requires the following discipline:

* Derived classes such as `D` must not expose a public constructor. Otherwise, `D`'s users could create `D` objects that don't invoke `PostInitialize`.
* Allocation is limited to `operator new`. `B` can, however, override `new` (see Items 45 and 46).
* `D` must define a constructor with the same parameters that `B` selected. Defining several overloads of `Create` can assuage this problem, however; and the overloads can even be templated on the argument types.

If the requirements above are met, the design guarantees that `PostInitialize` has been called for any fully constructed `B`-derived object. `PostInitialize` doesn't need to be virtual; it can, however, invoke virtual functions freely.

In summary, no post-construction technique is perfect. The worst techniques dodge the whole issue by simply asking the caller to invoke the post-constructor manually. Even the best require a different syntax for constructing objects (easy to check at compile time) and/or cooperation from derived class authors (impossible to check at compile time).

**References**: [\[Alexandrescu01\]](#Alexandrescu01) §3, [\[Boost\]](#Boost), [\[Dewhurst03\]](#Dewhurst03) §75, [\[Meyers97\]](#Meyers97) §46, [\[Stroustrup00\]](#Stroustrup00) §15.4.3, [\[Taligent94\]](#Taligent94)

### <a name="Sd-dtor"></a>Discussion: Make base class destructors public and virtual, or protected and nonvirtual

Should destruction behave virtually? That is, should destruction through a pointer to a `base` class be allowed? If yes, then `base`'s destructor must be public in order to be callable, and virtual otherwise calling it results in undefined behavior. Otherwise, it should be protected so that only derived classes can invoke it in their own destructors, and nonvirtual since it doesn't need to behave virtually.

##### Example

The common case for a base class is that it's intended to have publicly derived classes, and so calling code is just about sure to use something like a `shared_ptr<base>`:

    class Base {
    public:
        ~Base();                   // BAD, not virtual
        virtual ~Base();           // GOOD
        // ...
    };

    class Derived : public Base { /* ... */ };

    {
        unique_ptr<Base> pb = make_unique<Derived>();
        // ...
    } // ~pb invokes correct destructor only when ~Base is virtual

In rarer cases, such as policy classes, the class is used as a base class for convenience, not for polymorphic behavior. It is recommended to make those destructors protected and nonvirtual:

    class My_policy {
    public:
        virtual ~My_policy();      // BAD, public and virtual
    protected:
        ~My_policy();              // GOOD
        // ...
    };

    template<class Policy>
    class customizable : Policy { /* ... */ }; // note: private inheritance

##### Note

This simple guideline illustrates a subtle issue and reflects modern uses of inheritance and object-oriented design principles.

For a base class `Base`, calling code might try to destroy derived objects through pointers to `Base`, such as when using a `unique_ptr<Base>`. If `Base`'s destructor is public and nonvirtual (the default), it can be accidentally called on a pointer that actually points to a derived object, in which case the behavior of the attempted deletion is undefined. This state of affairs has led older coding standards to impose a blanket requirement that all base class destructors must be virtual. This is overkill (even if it is the common case); instead, the rule should be to make base class destructors virtual if and only if they are public.

To write a base class is to define an abstraction (see Items 35 through 37). Recall that for each member function participating in that abstraction, you need to decide:

* Whether it should behave virtually or not.
* Whether it should be publicly available to all callers using a pointer to `Base` or else be a hidden internal implementation detail.

As described in Item 39, for a normal member function, the choice is between allowing it to be called via a pointer to `Base` nonvirtually (but possibly with virtual behavior if it invokes virtual functions, such as in the NVI or Template Method patterns), virtually, or not at all. The NVI pattern is a technique to avoid public virtual functions.

Destruction can be viewed as just another operation, albeit with special semantics that make nonvirtual calls dangerous or wrong. For a base class destructor, therefore, the choice is between allowing it to be called via a pointer to `Base` virtually or not at all; "nonvirtually" is not an option. Hence, a base class destructor is virtual if it can be called (i.e., is public), and nonvirtual otherwise.

Note that the NVI pattern cannot be applied to the destructor because constructors and destructors cannot make deep virtual calls. (See Items 39 and 55.)

Corollary: When writing a base class, always write a destructor explicitly, because the implicitly generated one is public and nonvirtual. You can always `=default` the implementation if the default body is fine and you're just writing the function to give it the proper visibility and virtuality.

##### Exception

Some component architectures (e.g., COM and CORBA) don't use a standard deletion mechanism, and foster different protocols for object disposal. Follow the local patterns and idioms, and adapt this guideline as appropriate.

Consider also this rare case:

* `B` is both a base class and a concrete class that can be instantiated by itself, and so the destructor must be public for `B` objects to be created and destroyed.
* Yet `B` also has no virtual functions and is not meant to be used polymorphically, and so although the destructor is public it does not need to be virtual.

Then, even though the destructor has to be public, there can be great pressure to not make it virtual because as the first virtual function it would incur all the run-time type overhead when the added functionality should never be needed.

In this rare case, you could make the destructor public and nonvirtual but clearly document that further-derived objects must not be used polymorphically as `B`'s. This is what was done with `std::unary_function`.

In general, however, avoid concrete base classes (see Item 35). For example, `unary_function` is a bundle-of-typedefs that was never intended to be instantiated standalone. It really makes no sense to give it a public destructor; a better design would be to follow this Item's advice and give it a protected nonvirtual destructor.

**References**: [\[C++CS\]](#CplusplusCS) Item 50, [\[Cargill92\]](#Cargill92) pp. 77-79, 207, [\[Cline99\]](#Cline99) §21.06, 21.12-13, [\[Henricson97\]](#Henricson97) pp. 110-114, [\[Koenig97\]](#Koenig97) Chapters 4, 11, [\[Meyers97\]](#Meyers97) §14, [\[Stroustrup00\]](#Stroustrup00) §12.4.2, [\[Sutter02\]](#Sutter02) §27, [\[Sutter04\]](#Sutter04) §18

### <a name="Sd-noexcept"></a>Discussion: Usage of noexcept

???

### <a name="Sd-never-fail"></a>Discussion: Destructors, deallocation, and swap must never fail

Never allow an error to be reported from a destructor, a resource deallocation function (e.g., `operator delete`), or a `swap` function using `throw`. It is nearly impossible to write useful code if these operations can fail, and even if something does go wrong it nearly never makes any sense to retry. Specifically, types whose destructors may throw an exception are flatly forbidden from use with the C++ Standard Library. Most destructors are now implicitly `noexcept` by default.

##### Example

    class Nefarious {
    public:
        Nefarious()  { /* code that could throw */ }   // ok
        ~Nefarious() { /* code that could throw */ }   // BAD, should not throw
        // ...
    };

1. `Nefarious` objects are hard to use safely even as local variables:


        void test(string& s)
        {
            Nefarious n;          // trouble brewing
            string copy = s;      // copy the string
        } // destroy copy and then n

    Here, copying `s` could throw, and if that throws and if `n`'s destructor then also throws, the program will exit via `std::terminate` because two exceptions can't be propagated simultaneously.

2. Classes with `Nefarious` members or bases are also hard to use safely, because their destructors must invoke `Nefarious`' destructor, and are similarly poisoned by its poor behavior:


        class Innocent_bystander {
            Nefarious member;     // oops, poisons the enclosing class's destructor
            // ...
        };

        void test(string& s)
        {
            Innocent_bystander i; // more trouble brewing
            string copy2 = s;      // copy the string
        } // destroy copy and then i

    Here, if constructing `copy2` throws, we have the same problem because `i`'s destructor now also can throw, and if so we'll invoke `std::terminate`.

3. You can't reliably create global or static `Nefarious` objects either:


        static Nefarious n;       // oops, any destructor exception can't be caught

4. You can't reliably create arrays of `Nefarious`:


        void test()
        {
            std::array<Nefarious, 10> arr; // this line can std::terminate(!)
        }

    The behavior of arrays is undefined in the presence of destructors that throw because there is no reasonable rollback behavior that could ever be devised. Just think: What code can the compiler generate for constructing an `arr` where, if the fourth object's constructor throws, the code has to give up and in its cleanup mode tries to call the destructors of the already-constructed objects ... and one or more of those destructors throws? There is no satisfactory answer.

5. You can't use `Nefarious` objects in standard containers:


        std::vector<Nefarious> vec(10);   // this line can std::terminate()

    The standard library forbids all destructors used with it from throwing. You can't store `Nefarious` objects in standard containers or use them with any other part of the standard library.

##### Note

These are key functions that must not fail because they are necessary for the two key operations in transactional programming: to back out work if problems are encountered during processing, and to commit work if no problems occur. If there's no way to safely back out using no-fail operations, then no-fail rollback is impossible to implement. If there's no way to safely commit state changes using a no-fail operation (notably, but not limited to, `swap`), then no-fail commit is impossible to implement.

Consider the following advice and requirements found in the C++ Standard:

> If a destructor called during stack unwinding exits with an exception, terminate is called (15.5.1). So destructors should generally catch exceptions and not let them propagate out of the destructor. --[\[C++03\]](#Cplusplus03) §15.2(3)
>
> No destructor operation defined in the C++ Standard Library (including the destructor of any type that is used to instantiate a standard-library template) will throw an exception. --[\[C++03\]](#Cplusplus03) §17.4.4.8(3)

Deallocation functions, including specifically overloaded `operator delete` and `operator delete[]`, fall into the same category, because they too are used during cleanup in general, and during exception handling in particular, to back out of partial work that needs to be undone.
Besides destructors and deallocation functions, common error-safety techniques rely also on `swap` operations never failing -- in this case, not because they are used to implement a guaranteed rollback, but because they are used to implement a guaranteed commit. For example, here is an idiomatic implementation of `operator=` for a type `T` that performs copy construction followed by a call to a no-fail `swap`:

    T& T::operator=(const T& other) {
        auto temp = other;
        swap(temp);
        return *this;
    }

(See also Item 56. ???)

Fortunately, when releasing a resource, the scope for failure is definitely smaller. If using exceptions as the error reporting mechanism, make sure such functions handle all exceptions and other errors that their internal processing might generate. (For exceptions, simply wrap everything sensitive that your destructor does in a `try/catch(...)` block.) This is particularly important because a destructor might be called in a crisis situation, such as failure to allocate a system resource (e.g., memory, files, locks, ports, windows, or other system objects).

When using exceptions as your error handling mechanism, always document this behavior by declaring these functions `noexcept`. (See Item 75.)

**References**: [\[C++CS\]](#CplusplusCS) Item 51; [\[C++03\]](#Cplusplus03) §15.2(3), §17.4.4.8(3), [\[Meyers96\]](#Meyers96) §11, [\[Stroustrup00\]](#Stroustrup00) §14.4.7, §E.2-4, [\[Sutter00\]](#Sutter00) §8, §16, [\[Sutter02\]](#Sutter02) §18-19

## <a name="Sd-consistent"></a>Define Copy, move, and destroy consistently

##### Reason

 ???

##### Note

If you define a copy constructor, you must also define a copy assignment operator.

##### Note

If you define a move constructor, you must also define a move assignment operator.

##### Example

    class X {
        // ...
    public:
        X(const X&) { /* stuff */ }

        // BAD: failed to also define a copy assignment operator

        X(x&&) noexcept { /* stuff */ }

        // BAD: failed to also define a move assignment operator
    };

    X x1;
    X x2 = x1; // ok
    x2 = x1;   // pitfall: either fails to compile, or does something suspicious

If you define a destructor, you should not use the compiler-generated copy or move operation; you probably need to define or suppress copy and/or move.

    class X {
        HANDLE hnd;
        // ...
    public:
        ~X() { /* custom stuff, such as closing hnd */ }
        // suspicious: no mention of copying or moving -- what happens to hnd?
    };

    X x1;
    X x2 = x1; // pitfall: either fails to compile, or does something suspicious
    x2 = x1;   // pitfall: either fails to compile, or does something suspicious

If you define copying, and any base or member has a type that defines a move operation, you should also define a move operation.

    class X {
        string s; // defines more efficient move operations
        // ... other data members ...
    public:
        X(const X&) { /* stuff */ }
        X& operator=(const X&) { /* stuff */ }

        // BAD: failed to also define a move construction and move assignment
        // (why wasn't the custom "stuff" repeated here?)
    };

    X test()
    {
        X local;
        // ...
        return local;  // pitfall: will be inefficient and/or do the wrong thing
    }

If you define any of the copy constructor, copy assignment operator, or destructor, you probably should define the others.

##### Note

If you need to define any of these five functions, it means you need it to do more than its default behavior -- and the five are asymmetrically interrelated. Here's how:

* If you write/disable either of the copy constructor or the copy assignment operator, you probably need to do the same for the other: If one does "special" work, probably so should the other because the two functions should have similar effects. (See Item 53, which expands on this point in isolation.)
* If you explicitly write the copying functions, you probably need to write the destructor: If the "special" work in the copy constructor is to allocate or duplicate some resource (e.g., memory, file, socket), you need to deallocate it in the destructor.
* If you explicitly write the destructor, you probably need to explicitly write or disable copying: If you have to write a non-trivial destructor, it's often because you need to manually release a resource that the object held. If so, it is likely that those resources require careful duplication, and then you need to pay attention to the way objects are copied and assigned, or disable copying completely.

In many cases, holding properly encapsulated resources using RAII "owning" objects can eliminate the need to write these operations yourself. (See Item 13.)

Prefer compiler-generated (including `=default`) special members; only these can be classified as "trivial", and at least one major standard library vendor heavily optimizes for classes having trivial special members. This is likely to become common practice.

**Exceptions**: When any of the special functions are declared only to make them nonpublic or virtual, but without special semantics, it doesn't imply that the others are needed.
In rare cases, classes that have members of strange types (such as reference members) are an exception because they have peculiar copy semantics.
In a class holding a reference, you likely need to write the copy constructor and the assignment operator, but the default destructor already does the right thing. (Note that using a reference member is almost always wrong.)

**References**: [\[C++CS\]](#CplusplusCS) Item 52; [\[Cline99\]](#Cline99) §30.01-14, [\[Koenig97\]](#Koenig97) §4, [\[Stroustrup00\]](#Stroustrup00) §5.5, §10.4, [\[SuttHysl04b\]](#SuttHysl04b)

Resource management rule summary:

* [Provide strong resource safety; that is, never leak anything that you think of as a resource](#Cr-safety)
* [Never throw while holding a resource not owned by a handle](#Cr-never)
* [A "raw" pointer or reference is never a resource handle](#Cr-raw)
* [Never let a pointer outlive the object it points to](#Cr-outlive)
* [Use templates to express containers (and other resource handles)](#Cr-templates)
* [Return containers by value (relying on move or copy elision for efficiency)](#Cr-value-return)
* [If a class is a resource handle, it needs a constructor, a destructor, and copy and/or move operations](#Cr-handle)
* [If a class is a container, give it an initializer-list constructor](#Cr-list)

### <a name="Cr-safety"></a>Discussion: Provide strong resource safety; that is, never leak anything that you think of as a resource

##### Reason

Prevent leaks. Leaks can lead to performance degradation, mysterious error, system crashes, and security violations.

**Alternative formulation**: Have every resource represented as an object of some class managing its lifetime.

##### Example

    template<class T>
    class Vector {
    // ...
    private:
        T* elem;   // sz elements on the free store, owned by the class object
        int sz;
    };

This class is a resource handle. It manages the lifetime of the `T`s. To do so, `Vector` must define or delete [the set of special operations](???) (constructors, a destructor, etc.).

##### Example

    ??? "odd" non-memory resource ???

##### Enforcement

The basic technique for preventing leaks is to have every resource owned by a resource handle with a suitable destructor. A checker can find "naked `new`s". Given a list of C-style allocation functions (e.g., `fopen()`), a checker can also find uses that are not managed by a resource handle. In general, "naked pointers" can be viewed with suspicion, flagged, and/or analyzed. A complete list of resources cannot be generated without human input (the definition of "a resource" is necessarily too general), but a tool can be "parameterized" with a resource list.

### <a name="Cr-never"></a>Discussion: Never throw while holding a resource not owned by a handle

##### Reason

That would be a leak.

##### Example

    void f(int i)
    {
        FILE* f = fopen("a file", "r");
        ifstream is { "another file" };
        // ...
        if (i == 0) return;
        // ...
        fclose(f);
    }

If `i == 0` the file handle for `a file` is leaked. On the other hand, the `ifstream` for `another file` will correctly close its file (upon destruction). If you must use an explicit pointer, rather than a resource handle with specific semantics, use a `unique_ptr` or a `shared_ptr` with a custom deleter:

    void f(int i)
    {
        unique_ptr<FILE, int(*)(FILE*)> f(fopen("a file", "r"), fclose);
        // ...
        if (i == 0) return;
        // ...
    }

Better:

    void f(int i)
    {
        ifstream input {"a file"};
        // ...
        if (i == 0) return;
        // ...
    }

##### Enforcement

A checker must consider all "naked pointers" suspicious.
A checker probably must rely on a human-provided list of resources.
For starters, we know about the standard-library containers, `string`, and smart pointers.
The use of `span` and `string_span` should help a lot (they are not resource handles).

### <a name="Cr-raw"></a>Discussion: A "raw" pointer or reference is never a resource handle

##### Reason

To be able to distinguish owners from views.

##### Note

This is independent of how you "spell" pointer: `T*`, `T&`, `Ptr<T>` and `Range<T>` are not owners.

### <a name="Cr-outlive"></a>Discussion: Never let a pointer outlive the object it points to

##### Reason

To avoid extremely hard-to-find errors. Dereferencing such a pointer is undefined behavior and could lead to violations of the type system.

##### Example

    string* bad()   // really bad
    {
        vector<string> v = { "This", "will", "cause", "trouble", "!" };
        // leaking a pointer into a destroyed member of a destroyed object (v)
        return &v[0];
    }

    void use()
    {
        string* p = bad();
        vector<int> xx = {7, 8, 9};
        // undefined behavior: x may not be the string "This"
        string x = *p;
        // undefined behavior: we don't know what (if anything) is allocated a location p
        *p = "Evil!";
    }

The `string`s of `v` are destroyed upon exit from `bad()` and so is `v` itself. The returned pointer points to unallocated memory on the free store. This memory (pointed into by `p`) may have been reallocated by the time `*p` is executed. There may be no `string` to read and a write through `p` could easily corrupt objects of unrelated types.

##### Enforcement

Most compilers already warn about simple cases and have the information to do more. Consider any pointer returned from a function suspect. Use containers, resource handles, and views (e.g., `span` known not to be resource handles) to lower the number of cases to be examined. For starters, consider every class with a destructor as resource handle.

### <a name="Cr-templates"></a>Discussion: Use templates to express containers (and other resource handles)

##### Reason

To provide statically type-safe manipulation of elements.

##### Example

    template<typename T> class Vector {
        // ...
        T* elem;   // point to sz elements of type T
        int sz;
    };

### <a name="Cr-value-return"></a>Discussion: Return containers by value (relying on move or copy elision for efficiency)

##### Reason

To simplify code and eliminate a need for explicit memory management. To bring an object into a surrounding scope, thereby extending its lifetime.

**See also**: [F.20, the general item about "out" output values](#Rf-out)

##### Example

    vector<int> get_large_vector()
    {
        return ...;
    }

    auto v = get_large_vector(); //  return by value is ok, most modern compilers will do copy elision

##### Exception

See the Exceptions in [F.20](#Rf-out).

##### Enforcement

Check for pointers and references returned from functions and see if they are assigned to resource handles (e.g., to a `unique_ptr`).

### <a name="Cr-handle"></a>Discussion: If a class is a resource handle, it needs a constructor, a destructor, and copy and/or move operations

##### Reason

To provide complete control of the lifetime of the resource. To provide a coherent set of operations on the resource.

##### Example

    ??? Messing with pointers

##### Note

If all members are resource handles, rely on the default special operations where possible.

    template<typename T> struct Named {
        string name;
        T value;
    };

Now `Named` has a default constructor, a destructor, and efficient copy and move operations, provided `T` has.

##### Enforcement

In general, a tool cannot know if a class is a resource handle. However, if a class has some of [the default operations](#SS-ctor), it should have all, and if a class has a member that is a resource handle, it should be considered as resource handle.

### <a name="Cr-list"></a>Discussion: If a class is a container, give it an initializer-list constructor

##### Reason

It is common to need an initial set of elements.

##### Example

    template<typename T> class Vector {
    public:
        Vector(std::initializer_list<T>);
        // ...
    };

    Vector<string> vs { "Nygaard", "Ritchie" };

##### Enforcement

When is a class a container? ???

# <a name="S-tools"></a>Appendix D: Supporting tools

This section contains a list of tools that directly support adoption of the C++ Core Guidelines. This list is not intended to be an exhaustive list of tools
that are helpful in writing good C++ code. If a tool is designed specifically to support and links to the C++ Core Guidelines it is a candidate for inclusion.

### <a name="St-clangtidy"></a>Tools: [Clang-tidy](http://clang.llvm.org/extra/clang-tidy/checks/list.html)

Clang-tidy has a set of rules that specifically enforce the C++ Core Guidelines. These rules are named in the pattern `cppcoreguidelines-*`.

### <a name="St-cppcorecheck"></a>Tools: [CppCoreCheck](https://docs.microsoft.com/en-us/visualstudio/code-quality/using-the-cpp-core-guidelines-checkers)

The Microsoft compiler's C++ code analysis contains a set of rules specifically aimed at enforcement of the C++ Core Guidelines.

# <a name="S-glossary"></a>Glossary

A relatively informal definition of terms used in the guidelines
(based off the glossary in [Programming: Principles and Practice using C++](http://www.stroustrup.com/programming.html))

More information on many topics about C++ can be found on the [Standard C++ Foundation](https://isocpp.org)'s site.

* *ABI*: Application Binary Interface, a specification for a specific hardware platform combined with the operating system. Contrast with API.
* *abstract class*: a class that cannot be directly used to create objects; often used to define an interface to derived classes.
  A class is made abstract by having a pure virtual function or only protected constructors.
* *abstraction*: a description of something that selectively and deliberately ignores (hides) details (e.g., implementation details); selective ignorance.
* *address*: a value that allows us to find an object in a computer's memory.
* *algorithm*: a procedure or formula for solving a problem; a finite series of computational steps to produce a result.
* *alias*: an alternative way of referring to an object; often a name, pointer, or reference.
* *API*: Application Programming Interface, a set of functions that form the communication between various software components. Contrast with ABI.
* *application*: a program or a collection of programs that is considered an entity by its users.
* *approximation*: something (e.g., a value or a design) that is close to the perfect or ideal (value or design).
  Often an approximation is a result of trade-offs among ideals.
* *argument*: a value passed to a function or a template, in which it is accessed through a parameter.
* *array*: a homogeneous sequence of elements, usually numbered, e.g., `[0:max)`.
* *assertion*: a statement inserted into a program to state (assert) that something must always be true at this point in the program.
* *base class*: a class used as the base of a class hierarchy. Typically a base class has one or more virtual functions.
* *bit*: the basic unit of information in a computer. A bit can have the value 0 or the value 1.
* *bug*: an error in a program.
* *byte*: the basic unit of addressing in most computers. Typically, a byte holds 8 bits.
* *class*: a user-defined type that may contain data members, function members, and member types.
* *code*: a program or a part of a program; ambiguously used for both source code and object code.
* *compiler*: a program that turns source code into object code.
* *complexity*: a hard-to-precisely-define notion or measure of the difficulty of constructing a solution to a problem or of the solution itself.
  Sometimes complexity is used to (simply) mean an estimate of the number of operations needed to execute an algorithm.
* *computation*: the execution of some code, usually taking some input and producing some output.
* *concept*: (1) a notion, and idea; (2) a set of requirements, usually for a template argument.
* *concrete class*: class for which objects can be created using usual construction syntax (e.g., on the stack) and the resulting object behaves much like an `int` as it comes to copying, comparison, and such
(as opposed to a base class in a hierarchy).
* *constant*: a value that cannot be changed (in a given scope); not mutable.
* *constructor*: an operation that initializes ("constructs") an object.
  Typically a constructor establishes an invariant and often acquires resources needed for an object to be used (which are then typically released by a destructor).
* *container*: an object that holds elements (other objects).
* *copy*: an operation that makes two object have values that compare equal. See also move.
* *correctness*: a program or a piece of a program is correct if it meets its specification.
  Unfortunately, a specification can be incomplete or inconsistent, or can fail to meet users' reasonable expectations.
  Thus, to produce acceptable code, we sometimes have to do more than just follow the formal specification.
* *cost*: the expense (e.g., in programmer time, run time, or space) of producing a program or of executing it.
  Ideally, cost should be a function of complexity.
* *customization point*: ???
* *data*: values used in a computation.
* *debugging*: the act of searching for and removing errors from a program; usually far less systematic than testing.
* *declaration*: the specification of a name with its type in a program.
* *definition*: a declaration of an entity that supplies all information necessary to complete a program using the entity.
  Simplified definition: a declaration that allocates memory.
* *derived class*: a class derived from one or more base classes.
* *design*: an overall description of how a piece of software should operate to meet its specification.
* *destructor*: an operation that is implicitly invoked (called) when an object is destroyed (e.g., at the end of a scope). Often, it releases resources.
* *encapsulation*: protecting something meant to be private (e.g., implementation details) from unauthorized access.
* *error*: a mismatch between reasonable expectations of program behavior (often expressed as a requirement or a users' guide) and what a program actually does.
* *executable*: a program ready to be run (executed) on a computer.
* *feature creep*: a tendency to add excess functionality to a program "just in case."
* *file*: a container of permanent information in a computer.
* *floating-point number*: a computer's approximation of a real number, such as 7.93 and 10.78e-3.
* *function*: a named unit of code that can be invoked (called) from different parts of a program; a logical unit of computation.
* *generic programming*: a style of programming focused on the design and efficient implementation of algorithms.
  A generic algorithm will work for all argument types that meet its requirements. In C++, generic programming typically uses templates.
* *global variable*: technically, a named object in namespace scope.
* *handle*: a class that allows access to another through a member pointer or reference. See also resource, copy, move.
* *header*: a file containing declarations used to share interfaces between parts of a program.
* *hiding*: the act of preventing a piece of information from being directly seen or accessed.
  For example, a name from a nested (inner) scope can prevent that same name from an outer (enclosing) scope from being directly used.
* *ideal*: the perfect version of something we are striving for. Usually we have to make trade-offs and settle for an approximation.
* *implementation*: (1) the act of writing and testing code; (2) the code that implements a program.
* *infinite loop*: a loop where the termination condition never becomes true. See iteration.
* *infinite recursion*: a recursion that doesn't end until the machine runs out of memory to hold the calls.
  In reality, such recursion is never infinite but is terminated by some hardware error.
* *information hiding*: the act of separating interface and implementation, thus hiding implementation details not meant for the user's attention and providing an abstraction.
* *initialize*: giving an object its first (initial) value.
* *input*: values used by a computation (e.g., function arguments and characters typed on a keyboard).
* *integer*: a whole number, such as 42 and -99.
* *interface*: a declaration or a set of declarations specifying how a piece of code (such as a function or a class) can be called.
* *invariant*: something that must be always true at a given point (or points) of a program; typically used to describe the state (set of values) of an object or the state of a loop before entry into the repeated statement.
* *iteration*: the act of repeatedly executing a piece of code; see recursion.
* *iterator*: an object that identifies an element of a sequence.
* *ISO*: International Organization for Standardization. The C++ language is an ISO standard, ISO/IEC 14882. More information at [iso.org](http://iso.org).
* *library*: a collection of types, functions, classes, etc. implementing a set of facilities (abstractions) meant to be potentially used as part of more that one program.
* *lifetime*: the time from the initialization of an object until it becomes unusable (goes out of scope, is deleted, or the program terminates).
* *linker*: a program that combines object code files and libraries into an executable program.
* *literal*: a notation that directly specifies a value, such as 12 specifying the integer value "twelve."
* *loop*: a piece of code executed repeatedly; in C++, typically a for-statement or a `while`-statement.
* *move*: an operation that transfers a value from one object to another leaving behind a value representing "empty." See also copy.
* *mutable*: changeable; the opposite of immutable, constant, and invariable.
* *object*: (1) an initialized region of memory of a known type which holds a value of that type; (2) a region of memory.
* *object code*: output from a compiler intended as input for a linker (for the linker to produce executable code).
* *object file*: a file containing object code.
* *object-oriented programming*: (OOP) a style of programming focused on the design and use of classes and class hierarchies.
* *operation*: something that can perform some action, such as a function and an operator.
* *output*: values produced by a computation (e.g., a function result or lines of characters written on a screen).
* *overflow*: producing a value that cannot be stored in its intended target.
* *overload*: defining two functions or operators with the same name but different argument (operand) types.
* *override*: defining a function in a derived class with the same name and argument types as a virtual function in the base class, thus making the function callable through the interface defined by the base class.
* *owner*: an object responsible for releasing a resource.
* *paradigm*: a somewhat pretentious term for design or programming style; often used with the (erroneous) implication that there exists a paradigm that is superior to all others.
* *parameter*: a declaration of an explicit input to a function or a template. When called, a function can access the arguments passed through the names of its parameters.
* *pointer*: (1) a value used to identify a typed object in memory; (2) a variable holding such a value.
* *post-condition*: a condition that must hold upon exit from a piece of code, such as a function or a loop.
* *pre-condition*: a condition that must hold upon entry into a piece of code, such as a function or a loop.
* *program*: code (possibly with associated data) that is sufficiently complete to be executed by a computer.
* *programming*: the art of expressing solutions to problems as code.
* *programming language*: a language for expressing programs.
* *pseudo code*: a description of a computation written in an informal notation rather than a programming language.
* *pure virtual function*: a virtual function that must be overridden in a derived class.
* *RAII*: ("Resource Acquisition Is Initialization") a basic technique for resource management based on scopes.
* *range*: a sequence of values that can be described by a start point and an end point. For example, `[0:5)` means the values 0, 1, 2, 3, and 4.
* *recursion*: the act of a function calling itself; see also iteration.
* *reference*: (1) a value describing the location of a typed value in memory; (2) a variable holding such a value.
* *regular expression*: a notation for patterns in character strings.
* *regular*: a type that behaves similarly to built-in types like `int` and can be compared with `==`.
In particular, an object of a regular type can be copied and the result of a copy is a separate object that compares equal to the original. See also *semiregular type*.
* *requirement*: (1) a description of the desired behavior of a program or part of a program; (2) a description of the assumptions a function or template makes of its arguments.
* *resource*: something that is acquired and must later be released, such as a file handle, a lock, or memory. See also handle, owner.
* *rounding*: conversion of a value to the mathematically nearest value of a less precise type.
* *RTTI*: Run-Time Type Information. ???
* *scope*: the region of program text (source code) in which a name can be referred to.
* *semiregular*: a type that behaves roughly like an built-in type like `int`, but possibly without a `==` operator. See also *regular type*.
* *sequence*: elements that can be visited in a linear order.
* *software*: a collection of pieces of code and associated data; often used interchangeably with program.
* *source code*: code as produced by a programmer and (in principle) readable by other programmers.
* *source file*: a file containing source code.
* *specification*: a description of what a piece of code should do.
* *standard*: an officially agreed upon definition of something, such as a programming language.
* *state*: a set of values.
* *STL*: the containers, iterators, and algorithms part of the standard library.
* *string*: a sequence of characters.
* *style*: a set of techniques for programming leading to a consistent use of language features; sometimes used in a very restricted sense to refer just to low-level rules for naming and appearance of code.
* *subtype*: derived type; a type that has all the properties of a type and possibly more.
* *supertype*: base type; a type that has a subset of the properties of a type.
* *system*: (1) a program or a set of programs for performing a task on a computer; (2) a shorthand for "operating system", that is, the fundamental execution environment and tools for a computer.
* *TS*: [Technical Specification](https://www.iso.org/deliverables-all.html?type=ts), A Technical Specification addresses work still under technical development, or where it is believed that there will be a future, but not immediate, possibility of agreement on an International Standard. A Technical Specification is published for immediate use, but it also provides a means to obtain feedback. The aim is that it will eventually be transformed and republished as an International Standard.
* *template*: a class or a function parameterized by one or more types or (compile-time) values; the basic C++ language construct supporting generic programming.
* *testing*: a systematic search for errors in a program.
* *trade-off*: the result of balancing several design and implementation criteria.
* *truncation*: loss of information in a conversion from a type into another that cannot exactly represent the value to be converted.
* *type*: something that defines a set of possible values and a set of operations for an object.
* *uninitialized*: the (undefined) state of an object before it is initialized.
* *unit*: (1) a standard measure that gives meaning to a value (e.g., km for a distance); (2) a distinguished (e.g., named) part of a larger whole.
* *use case*: a specific (typically simple) use of a program meant to test its functionality and demonstrate its purpose.
* *value*: a set of bits in memory interpreted according to a type.
* *variable*: a named object of a given type; contains a value unless uninitialized.
* *virtual function*: a member function that can be overridden in a derived class.
* *word*: a basic unit of memory in a computer, often the unit used to hold an integer.

# <a name="S-unclassified"></a>To-do: Unclassified proto-rules

This is our to-do list.
Eventually, the entries will become rules or parts of rules.
Alternatively, we will decide that no change is needed and delete the entry.

* No long-distance friendship
* Should physical design (what's in a file) and large-scale design (libraries, groups of libraries) be addressed?
* Namespaces
* Avoid using directives in the global scope (except for std, and other "fundamental" namespaces (e.g. experimental))
* How granular should namespaces be? All classes/functions designed to work together and released together (as defined in Sutter/Alexandrescu) or something narrower or wider?
* Should there be inline namespaces (à la `std::literals::*_literals`)?
* Avoid implicit conversions
* Const member functions should be thread safe ... aka, but I don't really change the variable, just assign it a value the first time it's called ... argh
* Always initialize variables, use initialization lists for member variables.
* Anyone writing a public interface which takes or returns `void*` should have their toes set on fire. That one has been a personal favorite of mine for a number of years. :)
* Use `const`-ness wherever possible: member functions, variables and (yippee) `const_iterators`
* Use `auto`
* `(size)` vs. `{initializers}` vs. `{Extent{size}}`
* Don't overabstract
* Never pass a pointer down the call stack
* falling through a function bottom
* Should there be guidelines to choose between polymorphisms? YES. classic (virtual functions, reference semantics) vs. Sean Parent style (value semantics, type-erased, kind of like `std::function`)  vs. CRTP/static? YES Perhaps even vs. tag dispatch?
* should virtual calls be banned from ctors/dtors in your guidelines? YES. A lot of people ban them, even though I think it's a big strength of C++ that they are ??? -preserving (D disappointed me so much when it went the Java way). WHAT WOULD BE A GOOD EXAMPLE?
* Speaking of lambdas, what would weigh in on the decision between lambdas and (local?) classes in algorithm calls and other callback scenarios?
* And speaking of `std::bind`, Stephen T. Lavavej criticizes it so much I'm starting to wonder if it is indeed going to fade away in future. Should lambdas be recommended instead?
* What to do with leaks out of temporaries? : `p = (s1 + s2).c_str();`
* pointer/iterator invalidation leading to dangling pointers:

        void bad()
        {
            int* p = new int[700];
            int* q = &p[7];
            delete p;

            vector<int> v(700);
            int* q2 = &v[7];
            v.resize(900);

            // ... use q and q2 ...
        }

* LSP
* private inheritance vs/and membership
* avoid static class members variables (race conditions, almost-global variables)

* Use RAII lock guards (`lock_guard`, `unique_lock`, `shared_lock`), never call `mutex.lock` and `mutex.unlock` directly (RAII)
* Prefer non-recursive locks (often used to work around bad reasoning, overhead)
* Join your threads! (because of `std::terminate` in destructor if not joined or detached ... is there a good reason to detach threads?) -- ??? could support library provide a RAII wrapper for `std::thread`?
* If two or more mutexes must be acquired at the same time, use `std::lock` (or another deadlock avoidance algorithm?)
* When using a `condition_variable`, always protect the condition by a mutex (atomic bool whose value is set outside of the mutex is wrong!), and use the same mutex for the condition variable itself.
* Never use `atomic_compare_exchange_strong` with `std::atomic<user-defined-struct>` (differences in padding matter, while `compare_exchange_weak` in a loop converges to stable padding)
* individual `shared_future` objects are not thread-safe: two threads cannot wait on the same `shared_future` object (they can wait on copies of a `shared_future` that refer to the same shared state)
* individual `shared_ptr` objects are not thread-safe: different threads can call non-`const` member functions on *different* `shared_ptr`s that refer to the same shared object, but one thread cannot call a non-`const` member function of a `shared_ptr` object while another thread accesses that same `shared_ptr` object (if you need that, consider `atomic_shared_ptr` instead)

* rules for arithmetic

# Bibliography

* <a name="Abrahams01"></a>
  \[Abrahams01]:  D. Abrahams. [Exception-Safety in Generic Components](http://www.boost.org/community/exception_safety.html).
* <a name="Alexandrescu01"></a>
  \[Alexandrescu01]:  A. Alexandrescu. Modern C++ Design (Addison-Wesley, 2001).
* <a name="Cplusplus03"></a>
  \[C++03]:           ISO/IEC 14882:2003(E), Programming Languages — C++ (updated ISO and ANSI C++ Standard including the contents of (C++98) plus errata corrections).
* <a name="CplusplusCS"></a>
  \[C++CS]:           ???
* <a name="Cargill92"></a>
  \[Cargill92]:       T. Cargill. C++ Programming Style (Addison-Wesley, 1992).
* <a name="Cline99"></a>
  \[Cline99]:         M. Cline, G. Lomow, and M. Girou. C++ FAQs (2ndEdition) (Addison-Wesley, 1999).
* <a name="Dewhurst03"></a>
  \[Dewhurst03]:      S. Dewhurst. C++ Gotchas (Addison-Wesley, 2003).
* <a name="Henricson97"></a>
  \[Henricson97]:     M. Henricson and E. Nyquist. Industrial Strength C++ (Prentice Hall, 1997).
* <a name="Koenig97"></a>
  \[Koenig97]:        A. Koenig and B. Moo. Ruminations on C++ (Addison-Wesley, 1997).
* <a name="Lakos96"></a>
  \[Lakos96]:         J. Lakos. Large-Scale C++ Software Design (Addison-Wesley, 1996).
* <a name="Meyers96"></a>
  \[Meyers96]:        S. Meyers. More Effective C++ (Addison-Wesley, 1996).
* <a name="Meyers97"></a>
  \[Meyers97]:        S. Meyers. Effective C++ (2nd Edition) (Addison-Wesley, 1997).
* <a name="Meyers15"></a>
  \[Meyers15]:        S. Meyers. Effective Modern C++ (O'Reilly, 2015).
* <a name="Murray93"></a>
  \[Murray93]:        R. Murray. C++ Strategies and Tactics (Addison-Wesley, 1993).
* <a name="Stroustrup94"></a>
  \[Stroustrup94]:    B. Stroustrup. The Design and Evolution of C++ (Addison-Wesley, 1994).
* <a name="Stroustrup00"></a>
  \[Stroustrup00]:    B. Stroustrup. The C++ Programming Language (Special 3rdEdition) (Addison-Wesley, 2000).
* <a name="Stroustrup05"></a>
  \[Stroustrup05]:    B. Stroustrup. [A rationale for semantically enhanced library languages](http://www.stroustrup.com/SELLrationale.pdf).
* <a name="Stroustrup13"></a>
  \[Stroustrup13]:    B. Stroustrup. [The C++ Programming Language (4th Edition)](http://www.stroustrup.com/4th.html). Addison Wesley 2013.
* <a name="Stroustrup14"></a>
  \[Stroustrup14]:    B. Stroustrup. [A Tour of C++](http://www.stroustrup.com/Tour.html).
  Addison Wesley 2014.
* <a name="Stroustrup15"></a>
  \[Stroustrup15]:    B. Stroustrup, Herb Sutter, and G. Dos Reis: [A brief introduction to C++'s model for type- and resource-safety](https://github.com/isocpp/CppCoreGuidelines/blob/master/docs/Introduction%20to%20type%20and%20resource%20safety.pdf).
* <a name="SuttHysl04b"></a>
  \[SuttHysl04b]:     H. Sutter and J. Hyslop. "Collecting Shared Objects" (C/C++ Users Journal, 22(8), August 2004).
* <a name="SuttAlex05"></a>
  \[SuttAlex05]:      H. Sutter and  A. Alexandrescu. C++ Coding Standards. Addison-Wesley 2005.
* <a name="Sutter00"></a>
  \[Sutter00]:        H. Sutter. Exceptional C++ (Addison-Wesley, 2000).
* <a name="Sutter02"></a>
  \[Sutter02]:        H. Sutter. More Exceptional C++ (Addison-Wesley, 2002).
* <a name="Sutter04"></a>
  \[Sutter04]:        H. Sutter. Exceptional C++ Style (Addison-Wesley, 2004).
* <a name="Taligent94"></a>
  \[Taligent94]: Taligent's Guide to Designing Programs (Addison-Wesley, 1994).
