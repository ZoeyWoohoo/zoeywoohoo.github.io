---
title: "(转) 为什么 Flutter 会选择 Dart?"
date: 2018-08-22 23:00:43 +0800
category: [Flutter, Dart]
tags: Flutter Dart
reward: false
comment: false
excerpt: (本文转载自 InfoQ) 这是一篇很好的文章，看完之后就想转载一下做个备份，在原文基础上做了一些文本的格式优化。
---

> 本文转载自 InfoQ：[原文链接][link] —— 『[原文][alink]作者：[Wm Leler][wmleler] 译者：[王纯超][wcc] 审校：[覃云][ty]』

许多语言学家认为，一个人说的自然语言会[影响他们的思维方式](https://en.wikipedia.org/wiki/Linguistic_relativity)。这个理论适用于[计算机语言](https://en.wikipedia.org/wiki/Programming_language)吗？使用不同编程语言编程的程序员针对问题想出的解决方案经常完全不同。举一个极端的例子，为了程序结构更加清晰，计算机科学家[取消了 goto 语句](https://homepages.cwi.nl/~storm/teaching/reader/Dijkstra68.pdf) (这与小说[《1984》](https://en.wikipedia.org/wiki/Nineteen_Eighty-Four)中的极权主义领导者从自然语言中删除异端词语以消除[思维犯罪](https://en.wikipedia.org/wiki/Thoughtcrime)不太一样，但道理就是这样)。

这与 [Flutter](https://flutter.io/) 和 [Dart](https://www.dartlang.org/) 有什么关系？确实有关系。早期的 Flutter 团队评估了十多种语言，并选择了 Dart，因为它符合他们构建用户界面的方式。

Dart 是开发人员喜欢 Flutter 的一大原因。如以下[推文](https://twitter.com/Jordy_vD_/status/950277357734162432)：

<blockquote class="twitter-tweet" data-lang="zh-cn"><p lang="en" dir="ltr"><a href="https://twitter.com/flutterio?ref_src=twsrc%5Etfw">@flutterio</a> got me to look at <a href="https://twitter.com/dart?ref_src=twsrc%5Etfw">@dart</a>, and I’m glad I took it for a spin. <a href="https://twitter.com/hashtag/Dart?src=hash&amp;ref_src=twsrc%5Etfw">#Dart</a> is an awesome language, and <a href="https://twitter.com/hashtag/flutterio?src=hash&amp;ref_src=twsrc%5Etfw">#flutterio</a> takes it even further, to mobile devices &lt;3</p>&mdash; Jordy (@Jordy_vD_) <a href="https://twitter.com/Jordy_vD_/status/950277357734162432?ref_src=twsrc%5Etfw">2018年1月8日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

以下是使 Dart 成为 Flutter 不可或缺的一部分的特性：

- Dart 是 AOT(Ahead Of Time) 编译的，编译成快速、可预测的本地代码，使 Flutter 几乎都可以使用 Dart 编写。这不仅使 Flutter 变得更快，而且几乎所有的东西 (包括所有的小部件) 都可以定制。
- Dart 也可以 JIT(Just In Time) 编译，开发周期异常快，工作流颠覆常规 (包括 Flutter 流行的亚秒级有状态热重载)。
- Dart 可以更轻松地创建以 60fps 运行的流畅动画和转场。Dart 可以在没有锁的情况下进行对象分配和垃圾回收。就像 JavaScript 一样，Dart 避免了抢占式调度和共享内存 (因而也不需要锁)。由于 Flutter 应用程序被编译为本地代码，因此它们不需要在领域之间建立缓慢的桥梁 (例如，JavaScript 到本地代码)。它的启动速度也快得多。
- Dart 使 Flutter 不需要单独的声明式布局语言，如 JSX 或 XML，或单独的可视化界面构建器，因为 Dart 的声明式编程布局易于阅读和可视化。所有的布局使用一种语言，聚集在一处，Flutter 很容易提供高级工具，使布局更简单。
- 开发人员发现 Dart 特别容易学习，因为它具有静态和动态语言用户都熟悉的特性。

并非所有这些功能都是 Dart 独有的，但它们的组合却恰到好处，使 Dart 在实现 Flutter 方面独一无二。因此，没有 Dart，很难想象 Flutter 像现在这样强大。

![flutterdart.png](https://www.z4a.net/images/2018/08/22/flutterdart.png)

本文接下来将深入探讨使 Dart 成为实现 Flutter 的最佳语言的许多特性 (包括其标准库)。

## 编译和执行

> 如果你已经了解静态语言与动态语言、AOT 与 JIT 编译以及虚拟机等主题，可以跳过本节。

历史上，计算机语言分为两组：[静态语言](https://en.wikipedia.org/wiki/Compiled_language) (例如，Fortran 和 C，其中变量类型是在编译时静态指定的) 和[动态语言](https://en.wikipedia.org/wiki/Interpreted_language) (例如，Smalltalk 和 JavaScript，其中变量的类型可以在运行时改变)。静态语言通常编译成目标机器的本地机器代码 (或汇编代码) 程序，该程序在运行时直接由硬件执行。动态语言由解释器执行，不产生机器语言代码。

当然，事情后来变得复杂得多。[虚拟机](https://en.wikipedia.org/wiki/Virtual_machine) (VM) 的概念开始流行，它其实只是一个高级的解释器，用软件模拟硬件设备。虚拟机使语言移植到新的硬件平台更容易。因此，VM 的输入语言常常是[中间语言](https://en.wikipedia.org/wiki/Intermediate_representation#Intermediate_language)。例如，一种编程语言 (如 [Java](https://en.wikipedia.org/wiki/Java_%28programming_language%29)) 被编译成中间语言 ([字节码](https://en.wikipedia.org/wiki/Java_bytecode))，然后在 VM([JVM](https://en.wikipedia.org/wiki/Java_virtual_machine)) 中执行。

另外，现在有[即时 (JIT) 编译器](https://en.wikipedia.org/wiki/Just-in-time_compilation)。JIT 编译器在程序执行期间运行，即时编译代码。原先在程序创建期间 (运行时之前) 执行的编译器现在称为 [AOT 编译器](https://en.wikipedia.org/wiki/Ahead-of-time_compilation)。

一般来说，只有静态语言才适合 AOT 编译为本地机器代码，因为机器语言通常需要知道数据的类型，而动态语言中的类型事先并不确定。因此，动态语言通常被解释或 JIT 编译。

在开发过程中 AOT 编译，开发周期 (从更改程序到能够执行程序以查看更改结果的时间) 总是很慢。但是 AOT 编译产生的程序可以更可预测地执行，并且运行时不需要停下来分析和编译。AOT 编译的程序也更快地开始执行 (因为它们已经被编译)。

相反，JIT 编译提供了更快的开发周期，但可能导致执行速度较慢或时快时慢。特别是，JIT 编译器启动较慢，因为当程序开始运行时，JIT 编译器必须在代码执行之前进行分析和编译。研究表明，[如果开始执行需要超过几秒钟，许多人将放弃应用](https://www.google.com/search?q=slow%20startup%20times%20lead%20to%20abandonment)。

以上就是背景知识。将 AOT 和 JIT 编译的优点结合起来不是很棒吗？请继续阅读。

## 编译与执行 Dart

在创造 Dart 之前，Dart 团队成员在高级编译器和虚拟机上做了开创性的工作，包括动态语言 (如 JavaScript 的 [V8 引擎](https://en.wikipedia.org/wiki/Chrome_V8) 和 Smalltalk 的 [Strongtalk](https://en.wikipedia.org/wiki/Strongtalk)) 以及静态语言 (如用于 Java 的 [Hotspot 编译器](https://en.wikipedia.org/wiki/HotSpot))。他们利用这些经验使 Dart 在编译和执行方面非常灵活。

Dart 是同时非常适合 AOT 编译和 JIT 编译的少数语言之一 (也许是唯一的『主流』语言)。支持这两种编译方式为 Dart 和 (特别是) Flutter 提供了显著的优势。

JIT 编译在开发过程中使用，编译器速度特别快。然后，当一个应用程序准备发布时，它被 AOT 编译。因此，借助先进的工具和编译器，Dart 具有两全其美的优势：极快的开发周期、快速的执行速度和极短启动时间。

Dart 在编译和执行方面的灵活性并不止于此。例如，Dart 可以[编译成 JavaScript](https://webdev.dartlang.org/tools/dart2js)，所以浏览器可以执行。这允许在移动应用和网络应用之间重复使用代码。开发人员报告他们的移动和网络应用程序之间的[代码重用率高达 70％](https://medium.com/@matthew.smith_66715/why-we-chose-flutter-and-how-its-changed-our-company-for-the-better-271ddd25da60)。通过将 Dart 编译为本地代码，或者编译为 JavaScript 并将其与 [node.js](https://nodejs.org/en/) 一起使用，Dart 也可以在服务器上使用。

最后，[Dart 还提供了一个独立的虚拟机](https://www.dartlang.org/dart-vm/tools/dart-vm) (本质上就像解释器一样)，虚拟机使用 Dart 语言本身作为其中间语言。

Dart 可以进行高效的 AOT 编译或 JIT 编译、解释或转译成其他语言。Dart 编译和执行不仅非常灵活，而且速度特别快。

下一节将介绍 Dart 编译速度的颠覆性的例子。

## 有状态热重载

Flutter 最受欢迎的功能之一是其极速热重载。在开发过程中，Flutter 使用 JIT 编译器，通常可以在一秒之内重新加载并继续执行代码。只要有可能，应用程序状态在重新加载时保留下来，以便应用程序可以从停止的地方继续。

除非自己亲身体验过，否则很难理解在开发过程中快速 (且可靠) 的热重载的重要性。开发人员报告称，它改变了他们创建应用的方式，将其描述为像[将应用绘制成生活](https://github.com/zilongc/blog/issues/3)一样。

[以下是一位移动应用程序开发人员对 Flutter 热重载的评价](https://medium.com/@lets4r/the-fluture-is-now-6040d7dcd9f3)：

> 我想测试热重载，所以我改变了颜色，保存修改，结果…… 就喜欢上它了 ❤！  
> 这个功能真的很棒。我曾认为 Visual Studio 中编辑和继续 (Edit & Continue) 很好用，但这简直令人惊叹。有了这个功能，我认为移动开发者的生产力可以提高两倍。  
> 这对我来说真的是翻天覆地的变化。当我部署代码并花费很长时间时，我分心了，做了其他事情，当我回到模拟器/设备时，我就忘了想测试的内容。有什么比花 5 分钟将控件移动 2px 更令人沮丧？有了 Flutter，这不再存在。

Flutter 的热重载也使得尝试新想法或尝试替代方案变得更加容易，从而为创意提供了巨大的推动力。

到目前为止，我们讨论了 Dart 给开发人员带来的好处。下一节将介绍 Dart 如何使创建满足用户需求的顺畅的应用程序更加轻松。

## 避免卡顿

应用程序速度快很不错，但流畅则更加了不起。即使是一个超快的动画，如果它不稳定，也会看起来很糟糕。但是，防止[卡顿](https://afasterweb.com/2015/08/29/what-the-jank/)可能很困难，因为因素太多。Dart 有许多功能可以避免许多常见的导致卡顿的因素。

当然，像任何语言一样，Flutter 也可能写出来卡顿的应用程序；Dart 通过提高可预测性，帮助开发人员更好地控制应用程序的流畅性，从而更轻松地提供最佳的用户体验。

[效果怎样呢](https://code.tutsplus.com/tutorials/developing-an-android-app-with-flutter--cms-28270)？

以 60fps 运行，使用 Flutter 创建的用户界面的性能远远优于使用其他跨平台开发框架创建的用户界面。

不仅仅比跨平台的应用程序好，而且和最好的原生应用程序一样好：

> UI 像黄油一样顺滑…… 我从来没有见过这样流畅的 Android 应用程序。

## AOT 编译和『桥』

我们讨论过一个有助于保持顺畅的特性，那就是 Dart 能 AOT 编译为本地机器码。预编译的 AOT 代码比 JIT 更具可预测性，因为在运行时不需要暂停执行 JIT 分析或编译。

然而，AOT 编译代码还有一个更大的优势，那就是避免了『JavaScript 桥梁』。当动态语言 (如 JavaScript) 需要与平台上的本地代码互操作时，[它们必须通过桥进行通信](https://hackernoon.com/whats-revolutionary-about-flutter-946915b09514)，这会导致[上下文切换](https://medium.com/@talkol/performance-limitations-of-react-native-and-how-to-overcome-them-947630d7f440)，从而必须保存特别多的状态 (可能会存储到辅助存储)。这些[上下文切换](https://en.wikipedia.org/wiki/Context_switch)具有双重打击，因为它们不仅会减慢速度，还会导致严重的卡顿。

![jsbridge.png](https://www.z4a.net/images/2018/08/22/jsbridge.png)

注意：即使编译后的代码也可能需要一个接口来与平台代码进行交互，并且这也可以称为桥，但它通常比动态语言所需的桥快几个数量级。另外，由于 Dart 允许将小部件等内容移至应用程序中，因此减少了桥接的需求。

## 抢占式调度、时间分片和共享资源

大多数支持多个并发执行线程的计算机语言 (包括 Java、Kotlin、Objective-C 和 Swift) 都使用[抢占式](https://en.wikipedia.org/wiki/Preemption_%28computing%29)来切换线程。每个线程都被分配一个时间分片来执行，如果超过了分配的时间，线程将被上下文切换抢占。但是，如果在线程间共享的资源 (如内存) 正在更新时发生抢占，则会导致[竞态条件](https://en.wikipedia.org/wiki/Race_condition)。

竞态条件具有双重不利，因为它可能会导致严重的错误，包括应用程序崩溃并导致数据丢失，而且由于它取决于[独立线程的时序](https://en.wikipedia.org/wiki/Race_condition#Software)，所以它特别难以找到并修复。在调试器中运行应用程序时，竞态条件常常消失不见。

解决竞态条件的典型方法是使用[锁](https://en.wikipedia.org/wiki/Lock_%28computer_science%29)来保护共享资源，阻止其他线程执行，但锁本身可能导致卡顿，甚至[更严重的问题](https://en.wikipedia.org/wiki/Dining_philosophers_problem) (包括[死锁](https://en.wikipedia.org/wiki/Deadlock)和[饥饿](https://en.wikipedia.org/wiki/Starvation_%28computer_science%29))。

**Dart 采取了不同的方法来解决这个问题。Dart 中的线程称为 isolate，不共享内存，从而避免了大多数锁。isolate 通过在通道上传递消息来通信，这与 [Erlang](https://www.erlang.org/) 中的 actor 或 JavaScript 中的 Web Worker 相似。**

Dart 与 JavaScript 一样，是[单线程](https://en.wikipedia.org/wiki/Thread_%28computing%29#Single_threading)的，这意味着它根本不允许抢占。相反，线程显式让出 (使用 [async/await、Future](https://www.dartlang.org/tutorials/language/futures) 和 [Stream](https://www.dartlang.org/tutorials/language/streams)) CPU。这使开发人员能够更好地控制执行。单线程有助于开发人员确保关键功能 (包括动画和转场) 完成而无需抢占。这通常不仅是用户界面的一大优势，而且还是客户端——服务器代码的一大优势。

当然，如果开发人员忘记了让出 CPU 的控制权，这可能会延迟其他代码的执行。然而我们发现，忘记让出 CPU 通常比忘记加锁更容易找到和修复 (因为竞态条件很难找到)。

## 对象分配和垃圾回收

另一个严重导致卡顿的原因是垃圾回收。事实上，这只是访问共享资源 (内存) 的一种特殊情况，在很多语言中都需要使用锁。但在回收可用内存时，[锁会阻止整个应用程序运行](https://en.wikipedia.org/wiki/Tracing_garbage_collection#Stop-the-world_vs._incremental_vs._concurrent)。但是，Dart 几乎可以在没有锁的情况下执行垃圾回收。

Dart 使用先进的[分代垃圾回收和对象分配方案](https://en.wikipedia.org/wiki/Tracing_garbage_collection#Generational_GC_.28ephemeral_GC.29)，该方案对于分配许多短暂的对象 (对于 Flutter 这样的反应式用户界面来说非常完美，Flutter 为每帧重建不可变视图树) 都特别快速。Dart 可以用一个指针凹凸分配一个对象 (不需要锁)。这也会带来流畅的滚动和动画效果，而不会出现卡顿。

## 统一的布局

Dart 的另一个好处是，Flutter 不会从程序中拆分出额外的模板或布局语言，如 JSX 或 XML，也不需要单独的可视布局工具。以下是一个简单的 Flutter 视图，用 Dart 编写：

```dart
new Center(child:
  new Column(children: [
    new Text('Hello, World!'),
    new Icon(Icons.star, color: Colors.green),
  ])
)
```

Dart 编写的视图及其效果：

![viewofdart.png](https://www.z4a.net/images/2018/08/23/viewofdart.png)

注意，可视化这段代码产生的效果是多么容易 (即使你没有使用 Dart 的经验)。

[Dart 2](https://www.dartlang.org/dart-2) 即将发布，这将变得更加简单，因为 new 和 const 关键字变得可选，所以静态布局看起来像是用声明式布局语言编写的：

```dart
Center(child:
  Column(children: [
    Text('Hello, World!'),
    Icon(Icons.star, color: Colors.green),
  ])
)
```

然而，我知道你可能在想什么 —— 缺乏专门的布局语言怎么会被称为优势呢？但它确实是颠覆性的。以下是一名开发人员在一篇题为『[为什么原生应用程序开发人员应认真看待 Flutter](https://hackernoon.com/why-native-app-developers-should-take-a-serious-look-at-flutter-e97361a1c073)』的文章中写的内容。

> 在 Flutter 里，界面布局直接通过 Dart 编码来定义，不需要使用 XML 或模板语言，也不需要使用可视化设计器之类的工具。  
> 说到这里，大家可能会一脸茫然，就像我当初的反应一样。使用可视化工具不是更容易吗？如果把所有的逻辑都写到代码里不是会让事情变复杂吗？  
> 结果不然。天啊，它简直让我大开眼界。

首先是上面提到的热重载。

> 这比 Android 的 Instant Run 和任何类似解决方案不知道要领先多少年。对于大型的应用同样适用。如此快的速度，正是 Dart 的优势所在。  
> 实际上，可视化编辑器就变得多余了。我一点都不怀恋 XCode 的自动重布局。

Dart 创建的布局简洁且易于理解，而『超快』的热重载可立即看到结果。这包括布局的非静态部分

> 结果，在 Flutter 中进行布局要比在 Android/XCode 中快得多。一旦你掌握了它 (我花了几个星期)，由于很少发生上下文切换，因此会节省大量的开销。不必切换到设计模式，选择鼠标并开始点击，然后想是否有些东西必须通过编程来完成，如何实现等等。因为一切都是程序化的。而且这些 API 设计得非常好。它很直观，并且比自动布局 XML 更强大。

例如，下面是一个简单的列表布局，在每个项目之间添加一个分隔线 (水平线)，以编程方式定义：

```dart
return new ListView.builder(itemBuilder: (context, i) {
  if (i.isOdd) return new Divider();
  // rest of function
});
```

​​​​​​​在 Flutter 中，无论是静态布局还是编程布局，所有布局都存在于同一个位置。[新的 Dart 工具](https://groups.google.com/forum/#!topic/flutter-dev/lKtTQ-45kc4)，包括 Flutter Inspector 和大纲视图 (利用所有的布局定义都在代码里) 使复杂而美观的布局更加容易。

## Dart 是专有语言吗？

不，Dart (如 Flutter) 是完全开源的，具备清楚的许可证，同时也是 [ECMA 标准](https://www.ecma-international.org/publications/standards/Ecma-408.htm)的。Dart 在 Google 内外很受欢迎。在谷歌内部，它是增长最快的语言之一，并被 Adwords、Flutter、[Fuchsia](https://github.com/fuchsia-mirror) 和其他产品使用；在谷歌外部，Dart 代码库有超过 100 个外部提交者。

Dart 开放性的更好指标是 Google 之外的社区的发展。例如，我们看到来自第三方的关于 Dart(包括 Flutter 和 AngularDart) 的文章和视频源源不断，我在本文中引用了其中的一些内容。

除了 Dart 本身的外部提交者之外，[公共 Dart 包仓库](https://pub.dartlang.org/)中还有超过 3000 个包，其中包括 Firebase、Redux、RxDart、国际化、加密、数据库、路由、集合等方面的库。

## Dart 程序员难找吗？

如果没有很多程序员知道 Dart，找到合格的程序员会困难吗？显然不是。Dart 是一门难以置信的易学语言。事实上，已经了解 Java、JavaScript、Kotlin、C＃ 或 Swift 等语言的程序员几乎可以立即开始使用 Dart 进行编程。

一个程序员在名为『[为什么 Flutter 2018 年将起飞](https://codeburst.io/why-flutter-will-take-off-in-2018-bbd75f8741b0)』的文章中写到：

> [Dart](https://www.dartlang.org/) 是用于开发 Flutter 应用程序的语言，很易学。谷歌在创建简单、有文档记录的语言方面拥有丰富的经验，如 Go。到目前为止，对我来说，Dart 让我想起了 Ruby，很高兴能够学习它。它不仅适用于移动开发，也[适用于 Web 开发](https://webdev.dartlang.org/)。

另一篇关于 Flutter 和 Dart 的文章，题为『[为什么是Flutter而不是其他框架？](https://medium.com/@franzsilva/why-flutter-and-not-framework-x-or-better-yet-why-im-going-flutter-all-in-b484ecb25336)』

> Flutter 使用由 Google 创建的 Dart 语言，老实说，我不喜欢 C＃ 或 JAVA 这样的强类型语言，但我不知道 Dart 编写代码的方式有什么与众不同。但我觉得写起来很舒服。也许是因为它非常简单易学，而且非常直观。

Dart 通过广泛的用户体验研究和测试，专门设计得熟悉并易于学习。例如，在 2017 年上半年，Flutter 团队与八位开发人员一起进行了[用户体验研究](https://medium.com/google-design/how-i-do-developer-ux-at-google-b21646c2c4df)。我们给他们简短地介绍了 Flutter，然后给他们一个小时左右，创建了一个简单的视图。所有参与者都能够立即开始编程，即使他们以前从未使用过 Dart。他们专注于写响应式视图，而不是语言。[Dart 直接就能上手用了](https://www.dartlang.org/guides/get-started)。

最后，一位参与者 (在任务中进展得特别快) 没有提及任何有关该语言的内容，所以我们问他是否知道他正在使用哪种语言。他说不知道。语言不成问题；他在几分钟内就能用 Dart 编程。

学习新系统的难点通常不是学习语言，而是学习编写好代码的所有库、框架、工具、模式和最佳实践。Dart 库和工具格外出色，并且文档详尽。[有一篇文章宣称](https://hn.svelte.technology/item/15416892)：『意外之喜是，他们还极其爱护代码库，并且他们拥有我见过的最好的文档』。花费在学习 Dart 上的时间很容易通过学习其他东西节省的时间弥补。

作为直接证据，Google 内部的一个大型项目希望将其移动应用程序移植到 iOS。他们即将聘请一些 iOS 程序员，但转而决定尝试 Flutter。他们监测了让开发者上手 Flutter 需要多长时间。结果表明，程序员可以学会 Dart 和 Flutter，并在三周内达到高效率。相比之下，他们之前观察到仅仅让程序员上手 Android(更不用说他们必须聘用和培训 iOS 开发人员) 需要五个星期。

最后，一家将三种平台 (iOS、Android 和 Web) 上的大型企业应用程序都迁移到 Dart 的公司，有一篇文章『[我们为什么选择Flutter以及它如何改变我们的公司](https://medium.com/@matthew.smith_66715/why-we-chose-flutter-and-how-its-changed-our-company-for-the-better-271ddd25da60)』。他们的结论：

> 招人变得容易多了。无论他们是来自 Web、iOS 还是 Android，我们现在都希望接受最佳人选。  
> 现在我们拥有 3 倍的工作效率，因为我们所有的团队都集中在一个代码库上。  
> 知识共享达到前所未有的高度。

使用 Dart 和 Flutter 使他们的生产力提高到三倍。考虑到他们以前在做什么，这应该不会令人感到意外。与许多公司一样，它们利用不同的语言、工具和程序员为每个平台 (Web、iOS 和 Android) 构建独立的应用程序。切换到 Dart 意味着他们不再需要雇佣三种不同的程序员。而且他们很容易将现有的程序员转移到使用 Dart。

他们和其他人发现，一旦程序员开始使用 Flutter，他们就会[爱上 Dart](https://traversoft.com/2017/06/07/talk-on-dart-and-flutter/)。他们喜欢 Dart 的简洁和缺乏仪式。他们喜欢级联、命名参数、async/await 和 Stream 等[语言特性](https://medium.com/@lukeaf/flutter-doesnt-need-kotlin-or-anything-else-5773965d5905)。而最重要的是，他们喜欢 Dart 带来的 Flutter 功能（如热重载），以及 Dart 帮助他们构建的美丽、高性能的应用程序。

## Dart 2

在本文发表时，Dart 2 正在发布。Dart 2 专注于改善构建客户端应用程序的体验，包括加快开发人员速度、改进开发人员工具和类型安全。例如，Dart 2 具有坚实的类型系统和类型推理。

Dart 2 还使 new 和 const 关键字可选。这意味着可以在不使用任何关键字的情况下描述 Flutter 视图，从而减少混乱并且易于阅读。例如：

```dart
Widget build(BuildContext context) =>
  Container(
    height: 56.0,
    padding: EdgeInsets.symmetric(horizontal: 8.0),
    decoration: BoxDecoration(color: Colors.blue[500]),
    child: Row(
      ...
    ),
  );
```

​​​​​​​Dart 2 自动计算出所有的构造函数，并且 `padding:` 的值是一个常量。

## 秘诀在于专注

Dart 2 的改进集中在优化客户端开发。但 Dart 仍然是构建服务器端、桌面、嵌入式系统和其他程序的绝佳语言。

专注是一件好事。几乎所有持久受欢迎的语言都受益于非常专注。例如：

- C 是编写操作系统和编译器的系统编程语言。
- Java 是为嵌入式系统设计的语言。
- JavaScript 是网页浏览器的脚本语言。
- 即使是饱受非议的 PHP 也成功了，因为它专注于编写个人主页 (它的名字来源)。

另一方面，许多语言已经明确地尝试过 (并且失败了) 成为完全是通用的，例如 PL/1 和 Ada 等等。最常见的问题是，如果没有重点，这些语言就成了众所周知的厨房洗碗槽。

许多使 Dart 成为好的客户端语言的特性也使其成为更好的服务器端语言。例如，Dart 避免了抢占式多任务处理，这一点与服务器上的 Node 具有相同的优点，但是数据类型更好更安全。

编写用于嵌入式系统的软件也是一样的。Dart 能够可靠地处理多个并发输入是关键。

最后，Dart 在客户端上的成功将不可避免地引起用户对服务器上使用的更多兴趣 —— 就像 JavaScript 和 Node 一样。为什么强迫人们使用两种不同的语言来构建客户端——服务器软件呢？

## 结论

这对于 Dart 来说是一个激动人心的时刻。使用 Dart 的人喜欢它，而 Dart 2 中的新特性使其成为你工具库中更有价值的补充。如果你还没有使用过 Dart，我希望这篇文章为你提供了有关 Dart 的新特性的有价值的信息，并且你会试一试 Dart 和 Flutter。


[link]: http://www.infoq.com/cn/articles/why-flutter-uses-dart
[alink]: https://hackernoon.com/why-flutter-uses-dart-dd635a054ebf
[wmleler]: https://hackernoon.com/@wmleler1
[wcc]: http://www.infoq.com/cn/profile/%E7%8E%8B%E7%BA%AF%E8%B6%85
[ty]: http://www.infoq.com/cn/profile/%E8%A6%83%E4%BA%91
