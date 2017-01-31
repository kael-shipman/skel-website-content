title: Home
author: Kael Shipman
contentClass: page
dateCreated: 2017-01-15

### Welcome to Skel

Skel is a library of standard PHP component interfaces that can be used by developers to build interoperable components and application frameworks. Its primary initial goal was to allow me to build my own custom application framework, but in a way that would allow others to a) easily understand and contribute to my projects, and b) use parts of my projects with their own or other third party implementations of the other parts. Thus, primarily, it is meant to be a way that developers can agree on how certain components should interact without forcing each other to use specific implementations of those components.

There's actually another project with this very same goal: [PHP-FIG](http://www.php-fig.org/), and more specifically FIG's [PHP Standards Recommendations](http://www.php-fig.org/psr/). While I applaud this development and am excited to someday to contribute to it, I personally wanted a sort of proving ground first. The process of standards development is long and highly bureaucratic. I needed a way to both quickly publish my best shot at some really unexciting interfaces (i.e., stuff that it's unlikely others would really care that much about), and also mash my interface proposals around in real-world use-cases to see if they were the best way forward before suggesting them as standards.

Thus, I conceived Skel as a sort of "beta world" for my own personal proposals to PHP-FIG PSRs, and I'm publishing it in a way that will allow others to use it, add to it, and change it.

### But is it a framework?

Kind of. What I needed when I defined the initial Skel interfaces was a framework. I wanted something that was tiny, bloat-free and that made sense to me. Thus, the interfaces currently defined by Skel pertain to the construction of web application micro-frameworks and related components. This is far from the only use case, though. In the same way that an industry develops a common vocabulary with which to develop ideas, the coding community must have a common set of interfaces that represent the problems we're trying to solve with the components we build. There are countless other interfaces that need to be designed, and Skel can be the home for any of these.

### So... This framework you built. Can I use it?

The truth is, the world does not need another web application framework. Yes, you can feel free to use it, and I'll certainly be using it for my personal projects, but it's also an experiment whose goals are much more personal exploration and much less real-world utility. While it attempts to address some very specific issues (detailed in [Chapter 1](/docs/01-conceptual-overview) of the documentation), I'm not sure that it actually will in the end. Furthermore, it may introduce new issues that may or may not negate any benefit it creates. The initial beta release has made me optimistic about its prospects for my personal projects, but unfortunately, I can't ring any victory bells yet. There is much yet to be proven, and only time will tell whether the framework really has anything to offer.

### Source

Skel is meant to be a dumping ground for interface prototypes. The idea is that when we develop a `User` class, for example, we should be implementing an interface for `User` that the community has more or less agreed on and can also implement. What I've found, though, is that those interfaces often don't exist. We simply implement a `User` class without binding it to any specific interface at all, thus condemning any future users of our `User` class to use only our class and derivatives.

What I dream is that programmers **always use interfaces**, and that those interfaces at least start their lives in Skel, so that we may have a standard and updateable interface library to work with moving forward. Eventually, these interfaces should all move to PSR, but until they do, I want easy access to them.

With that in mind, feel free to fork and modify the official [github repo](https://github.com/kael-shipman/skelphp-header). Also, note that there are a number of other "skelphp\*" sibling repos in my account. These are my own implementations of the Skel interfaces and are what I consider the "official" implementations, from which I've built the Skel Framework. You can also feel free to fork and create pull requests for these, but the point of Skel is not so much to make you use my implementations, but to allow you to make your own implementations and still have them be compatible with mine.

