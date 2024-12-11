# 动态作用域是一种效应(Effect)，隐式参数是一种协效应(CoEffect)

很长一段时间里，我认为[隐式参数(implicit parameters)](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#implicit-parameters)和动态作用域基本上是一样的，因为它们都可以用来解决类似的问题（例如所谓的“配置问题”，你需要将一些配置传递到嵌套的函数定义体中，而不需要显式地定义它们）。但隐式参数有[不应使用](https://www.reddit.com/r/haskell/comments/6gz4w5/whats_wrong_with_implicitparams/)（[建议使用reflection代替](https://www.reddit.com/r/haskell/comments/5xqozf/implicit_parameters_vs_reflection/dek9eqg/)），而通过reader monad实现的动态作用域是一个有用且理解透彻的构造除了你必须将一切都单子化(也就是得是monad的instance)这一点。

为什么会有这种区别？

[Oleg](http://okmij.org/ftp/Computation/dynamic-binding.html#implicit-parameter-neq-dynvar)指出隐式参数并不是真正的动态作用域，并给出了一个Lisp和Haskell不一致的例子。而且你甚至不希望在Haskell中有Lisp的行为：如果你考虑动态作用域的操作概念（沿着堆栈向上走，直到找到动态变量的绑定点），它与惰性求值不太兼容，因为一个访问动态变量的thunk将在程序执行的某个不可预测的时间点被强制执行。你真的不想去推理thunk将在哪里执行以知道它的动态变量将如何绑定，那样会导致混乱。但在严格语言中，似乎没有人会对动态作用域应该发生什么感到困惑（[好吧](https://blog.klipse.tech/clojure/2018/12/25/dynamic-scope-clojure.html)，[大部分时候](https://stuartsierra.com/2013/03/29/perils-of-dynamic-scope)--稍后会详细讨论）。

事实证明，研究界已经发现区别在于隐式参数是一种协效应。我相信这是在[协效应：上下文依赖的统一静态分析](http://tomasp.net/academic/papers/coeffects/coeffects-icalp.pdf)中首次观察到的（一个更现代的介绍在[协效应：上下文依赖计算的演算](https://www.doc.ic.ac.uk/~dorchard/publ/coeffects-icfp14.pdf)中；一个更Haskell化的介绍可以在[在Haskell中嵌入效应系统](http://tomasp.net/academic/papers/haskell-effects/haskell-effects.pdf)中找到）。

关键点在于，对于某些协效应（即隐式参数），按名调用的归约保留了类型和协效应，因此隐式参数不会像动态作用域（效应）那样爆炸。这些行为必然不同！类型类也是协效应，这就是为什么在Haskell中现代使用隐式参数时明确承认这一点（例如，在reflection包中）。

在2020年的ICFP上，我被指向了一份关于Koka中[隐式值和函数](https://www.microsoft.com/en-us/research/uploads/prod/2019/03/implicits-tr-v2.pdf)的有趣技术报告，这是动态作用域的新变化。我发现自己在想Haskell的隐式参数是否可以从这项工作中学到一些东西。隐式值做出了一个好的选择，即在顶层全局定义隐式值，以便它们可以参与正常的模块命名空间，而不是一个无命名空间的动态作用域名称袋（这也是reflection相对于隐式参数的一个改进）。但实际上，在我看来，隐式函数是从隐式参数中借鉴了一些东西！

隐式函数的重大创新在于它解析了函数中的所有动态引用（不仅是词法上的，而且是所有进一步的动态调用）到词法作用域（函数定义时的动态作用域），生成一个不依赖于隐式值的函数（即，没有_效应_表明隐式值必须在函数调用时定义）。这正是隐式参数let ?x = ...绑定所做的事情，实际上是在定义时直接填充隐式函数的字典，而不是等待。非常上下文化！（当然，Koka使用代数效应实现了这一点，并通过非常简单的翻译达到了正确的语义）。结果不完全是动态作用域，但正如技术报告所说，它带来了更好的抽象。

很难看到隐式值/函数如何回到Haskell中，至少没有一些序列构造（例如单子）在周围徘徊。尽管隐式函数的行为很像隐式参数，其余的动态作用域（包括隐式函数本身的绑定）只是老式的有影响（非协效应）的动态作用域。而你不能在Haskell中做到这一点，而不破坏在beta归约和eta扩展下的类型保留。Haskell别无选择，只能 _走到底_，一旦你超越了隐式参数的明显问题（reflection解决了这些问题），事情似乎大多能解决。
