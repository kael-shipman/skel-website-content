title: Home
author: Kael Shipman
contentClass: page
dateCreated: 2017-01-15

### Welcome to Skel

Skel is a PHP web applications meta-framework designed for developers. It was built in response to frustration with the extraordinary bloat of modern web applications frameworks, the resultant dependency hell, dealing with hard-to-follow "magic" that made it feel like the framework was working against you, and the tendency of frameworks to become a completely new vocabulary for developers to learn on top of the language the frameworks are built in. It was also built in an attempt to enforce a meaningful separation of concerns that would result in true code reusability, whether the user interface is a web page, a CLI, or a Desktop GUI.

Skel is an experiment. While it attempts to address the aforementioned issues and more, I'm not sure that it actually will in the end. Furthermore, it may introduce new issues that may or may not negate any benefit it creates. The initial beta release has made me optimistic about its prospects for my personal projects, but unfortunately, I can't ring any victory bells yet. There is much yet to be proven, and only time will tell whether the framework really has anything to offer.

So here I present its home. Read on to learn more about it.

### Did We Really Need Another Framework?

No. Skel does not purport to solve any major problems that people are experiencing in the field today. As I mentioned before, it does solve some very specific problems that I've run into a lot, but I can't say I've heard others complain too much about these things. They're problems that may simply be unique to the way I approach building websites and apps.

I built Skel purely as an experiment to satisfy my curiosity about a few things and as an excuse to practice a few useful concepts that I hadn't kept up with over the years (mainly TDD, PHP's more modern object-oriented programming features, and inline documentation).

So no, I don't expect Skel to compete among the world's web frameworks. I personally find it useful as a very, very lightweight way to build basic applications quickly and in a way that will scale well with complexity, but I won't pretend that it will be useful to others. Take it or leave it!

### Quickstart: Basic Concepts

There are a few things to keep in mind when considering Skel. First, Skel was developed under the following principles:

  * Your personal logic is valid and your way is one of many potentially "right" ways of doing things. You should be allowed to rely on your own implementations if you want to, without being forced to use third-party code that you don't trust or like.
  * "Magic" is really confusing. "Explicit" is preferable, even at the cost of complexity.
  * You should never have to hack through the framework to get what you want -- rather, it should either provide a useful starting point for you or stay out of your way.
  * You should not be required to include countless dependencies that you don't need, want or value.

Given the above, here are some important ideas that should help you understand Skel and how to use it:

 1. The actual framework itself is intended to be represented entirely by the interfaces defined in the [Header package](https://github.com/kael-shipman/skelphp-header). This package is supposed to be the only actual requirement for the framework, even though it doesn't do anything. The point is that you, the developer, may opt to use pre-built implementations of the Skel interfaces (either the official Skel implementations or others), or implement them yourself. This was one of the goals of the framework, in recognition of the fact that each developer has his own system of logic and should be able to use a framework that's familiar to all, but built to the individual specification of each developer. Thus, the header package presents a "familiar" (i.e., well-documented) set of defined interfaces, leaving the developer to implement them to his or her own specification. (Note that I'm working on composing a small collection of meta-packages for useful things like "basic blog", "advanced blog", "basic single page app", "advanced single page app", etc....)
 1. The framework assumes that many of the pre-built class implementations will be extended for your specific application. To accommodate this, the type dependencies in method calls for pre-built classes are for interfaces, not class implementations. Some interfaces (like Db and Cms, for exmample) contain almost no immediately useful methods (more on this [here](/docs/theory-in-depth#datalayer)). Given this, you are encouraged to extend at will, and to bend any pre-built classes to your needs.
 1. One of the central components of the framework is (aptly) the `Component` interface. This interface defines a class that keeps an arbitrary collection of data elements, some of which may also be Components (a classic tree structure), and then presents a method for rendering these elements into a Template (which may optionally be a program that outputs directly to screen, changes elements that are already on screen, or is simply a string into which the Component's elements are substituted). This is the "engine" that encourages code reusability. Since many framework methods are expecting objects that implement the Component and derivative interfaces, custom logic that you write for your own application -- preferably packaged into discrete and reasonably-documented libraries -- can both receive and return Components and thus function effectively outside of the context of your specific application. (More on this [here](/docs/theory-in-depth#component-interface).)

To learn more, take a look at the [Official Documentation](/docs).

### Source

Skel is a humble experiment, but it is still open source. If you'd like to mess with it, fork it, whatever, check out the official repos on [github](https://github.com/kael-shipman). (As noted above, the "framework" is actually the [Skel Header package](https://github.com/kael-shipman/skelphp-header), all other `skelphp*` packages are my own implementations of the interfaces that comprise the framework.)

