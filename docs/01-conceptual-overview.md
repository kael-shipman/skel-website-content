title: Chapter 1: Conceptual Overview
contentClass: page
dateCreated: 2017-01-25
author: Kael Shipman

### What is Skel?

Skel is a library of standard PHP component interfaces that can be used by developers to build interoperable components and application frameworks. Its primary initial goal was to allow me to build my own custom application framework, but in a way that would allow others to a) easily understand and contribute to my projects, and b) use parts of my projects with their own or other third party implementations of the other parts. Thus, primarily, it is meant to be a way that developers can agree on how certain components should interact without forcing each other to use specific implementations of those components.

There's actually another project with this very same goal: [PHP-FIG](http://www.php-fig.org/), and more specifically FIG's [PHP Standards Recommendations](http://www.php-fig.org/psr/). While I applaud this development and am excited to someday to contribute to it, I personally wanted a sort of proving ground first. The process of standards development is long and highly bureaucratic. I needed a way to both quickly publish my best shot at some really unexciting interfaces (i.e., stuff that it's unlikely others would really care that much about), and also mash my interface proposals around in real-world use-cases to see if they were the best way forward before suggesting them as standards.

Thus, I conceived Skel as a sort of "beta world" for my own personal proposals to PHP-FIG PSRs, and I'm publishing it in a way that will allow others to use it, add to it, and change it.

Skel can also be thought of as a meta framework -- that is, it is *not* a fully-built framework in itself so much as a set of interfaces that define the *interactions* between components of a framework. There is an official set of implementations of these interfaces, but they are not meant to be the focal point of Skel. Skel is meant to allow programmers to create their own reusable components and frameworks in a way that will be compatible with those of others. It is a common ground for component interactions, allowing each programmer to build a system that makes sense to him or her -- that he or she can trust and understand deeply -- without departing from a common set of agreed-upon component interactions.

### For whom was Skel built?

Skel is intended to be used by programmers as a foundation for building their own application frameworks and reusable component libraries. People who use Skel should be intimately familiar with PHP, object-oriented programming, and strict typing in general.

Skel was not intended to be a plug-and-play or set-it-and-forget-it framework. It prioritizes power over simplicity, so the assumption is that those who use it are capable of understanding how it works and how to use it to get exactly what they want. Since it only attempts to outline functionality, rather than making assumptions about how you want the functionality implemented, it invariably requires the authoring of significant amounts of logic. This is not a task for beginners.

### Why was Skel built? What problems does it attempt to solve?

Thousands of people have created `Uri` classes; thousands have created `Router` classes; thousands have created `User` classes and many other such classes. Yet rarely, if ever, do these classes implement a published *interface*, and even more rarely do class methods that call for them require the interface as type, rather than the class itself. So the world continues in its comedy of circles, with generation after generation of programmers all implementing incompatible classes that do the same thing, and all requiring their own classes as dependencies when anyone else's class would do.

This was the main reason for building Skel. Skel is an attempt to define a framework (and more generally a class ecosystem) entirely in interfaces. No class method in any implementation of a Skel interface should ever require a specific class; rather, they should always require interface implementations. Doing so will create a landscape where a) programmers are creating their own personal *versions* of components that can be subbed in transparently for other versions, and b) dependencies will no longer pile up like corpses in a zombie apocalypse.

There are other benefits to Skel, though. On top of alleviating bloat and dependency hell, the clearly defined way in which Skel components interact attempts to prevent the frustrating "magic" that often makes it feel like a framework is working against you. It was also built in an attempt to enforce a meaningful separation of concerns that would result in true code reusability, whether the user interface is a web page, a CLI, or a Desktop GUI.

### How does Skel fit into the world?

As stated above, Skel is an interface library. It could eventually become a relatively large interface library, but the point of having such a library available is pure compatibility. In an ideal world, PHP might have this library built in, such that when I need a `User` class, for example, I can either quickly implement the standard PHP `User` interface myself, or I can find the best 3rd-party implementation and incorporate it as a project dependency. Importantly, though, if I ever want a different `User` implementation, the fact that I used a class that implements the standard `User` interface will allow me to quickly find and incorporate a different implementation. (This might happen if, for example, I decided to use a class on which development eventually stopped, and I wanted to switch to a class with more active development.)

So Skel fits into the world by being a starting point for common interface development. Every single class I create from now on will implement an interface defined in the official [Skel Header Package](https://github.com/kael-shipman/skelphp-header). Eventually, if others begin to see the value in this, they might extend my interfaces, propose modifications to them, or create new ones. This can all be done via standard fork and pull request, allowing for a community-managed common interface library that any project can incorporate without putting on significant weight.

Ideally, the interfaces in Skel that reach maturity (i.e., that eventually stop changing) may be proposed to the PHP-FIG PSR project as official recommended interfaces. Thus, Skel will serve as a usable beta project for interfaces in development and as a proof of concept when the time comes to submit interfaces as PSRs.

