title: Theory
author: Kael Shipman
contentClass: page
dateCreated: 2017-01-15
dateExpired: 2017-02-01

Skel's theoretical foundations are a bit complex. The premise for its creation was that I was unable to find a framework that both felt intuitive to me and was not heinously bloated and over-dependent. I didn't have a firm metric in mind, but it seemed inconceivable to me that it would take more than about a megabyte of code to create an app that just served up my little blog. I didn't need a complex login system, and I didn't need a GUI for content authoring and management.

I did want something that was well-structured, reusable, logical, documented and explainable, though. And I wanted something that I could easily add powerful custom functionality to.

### Thus, Skel.

#### Power Over Ease

The first decision was that Skel would not cater to the average user. Instead, it would be engineered specifically for programmers who already knew what they were doing and wanted a good starting point for their projects. (Contrast this with platforms like Wordpress and Bolt, or even Rails.) Certainly, one could extend Skel to create a more complex, Wordpress-like framework with a lot of user-facing features, but the point of Skel itself was to start much closer to the beginning. In the true sense of a framework, it attempts to provide friction-free *means* for functionality, keeping that functionality ordered in a logical and expected way, without actually doing the work (and imposing the overhead) of implementing the functionality itself.

#### Minimal Dependencies

To this end, Skel effectively "ships" without any functionality at all -- it is just a collection of interfaces. This makes it dependency-free. While to use it you do need *some* class that implements the right interfaces, you don't need any *specific* classes, and thus it doesn't require any dependencies. (Of course, all of the interfaces have an "official" implementation, but the whole point is to give you the freedom to use the framework without using my specific implementations of the functionality.)

#### Minimal Bloat

Building Skel as a collection of interfaces not only minimized dependencies, but also bloat (i.e., unneeded features). When using other frameworks, I often found that I didn't need this or that gigantic component, and I resented having to include it in my codebase anyway. With Skel, you can literally stub 100% of the framework out by implementing the interfaces yourself with empty methods. Of course, it won't do anything, but that's the point -- if you don't need it to do anything, then who am I to insist that you accept all the functionality I hoped you would need?

### Skel's Conventions

As might be guessed by its size, Skel is more of a best-practice framework than a code framework. Much of what makes an application "Skel-compatible" is actually the *way* it's put together and the choices that are made about where functionality should go. In creating Skel, I've tried to enforce some of these concepts using PHP's type hinting. Unfortunately, though, many of them cannot be enforced using type alone, especially since so much of the end functionality of the app is written by the specific programmer who's working on it, and not by me or others working on the Skel framework. Thus, it's crucially important to understand the theoretical composition of a Skel app when considering using the framework. Below is an overview of the best practices that Skel relies on; a more in-depth discussion will eventually be available on the Skel Conventions page (yet to be created).

#### Libraries vs Applications

The difference between a library and an application was one of the most difficult elements for me to clarify for myself. While I suspect this is covered in CompSci 101, I have yet to take a computer science course, so remain ignorant of many of the useful nuggets of information therein contained, including this one.

That said, I do feel that I've done a decent job of clarifying the issue. To me, a library is a related set of classes and/or functions that are used to transform data. This may be a calculator library that parses an input string and transforms it into a final quantity, with more granular methods for operations like addition, subtraction, multiplication, etc; it may be a content parser library that accepts a string of content and transforms it into known fields; it may be an encryption library that accepts a string and a key and outputs the encrypted result.... Whatever the case, a library receives data and returns data, and optionally throws exceptions. It does not interact directly with users.

An application, then, is a wrapper program that *does* interact with users. Its job is to solicit and accept input, then optionally output stuff that the user requested or that will help inform the user about what's going on. An application will usually use one or more libraries to get and manipulate data that pertains to the user's request, but then it will transform all data that it retrieves into useful content for display to the user. As a result of this, "applications" are usually small, serving simply to coordinate interactions between libraries and users.

It should be noted that I tried for a while to mash all this into the MVC paradigm and simply didn't find it to be a very useful one. The idea of a model is too simple, and a controller that serves only as a bridge between user interactions in the view and data manipulations in the model is just a little too much of a stretch. So I opted not to use the MVC paradigm and instead to broaden the scope and deal with "libraries" and "applications" passing around data objects.

Anyway, the challenge in all this is to ensure any library always returns data in a format any application can use. Since an application may use data output from one library as input for another, it's important that the data remain in raw form for as long as possible. Furthermore, since libraries don't interact directly with users, it wouldn't make sense for them to transform their data into user-readable form; rather, the application should be responsible for this.

This is a detail that I've often found lacking in other frameworks. Silex, for example, passes around Response objects; yet a Response object relies on a string "content" property. To use this, you must reduce your data to string before proceeding. In Rails, the situation is similar: your controllers should "render" a string as output to the user, and you can choose a number of formats in which to do this.

**Best Practice:** The Skel solution to this, then, is to pass around `Components` for as long as possible (see next section). Your libraries should definintely return `Components`, may optionally accept `Components` as arguments, and your application "controller" methods should return `Components`. A `Component` should be rendered to string using its `render` method at the last possible opportunity to do so (the official Skel `App` implementation does this when it converts the response of its route (a `Component`) into a `Response` object for output to the browser).

#### The `Component` Interface -- The Reusability Engine {#component-interface}

The Skel `Component` interface simply defines an array-like structure with an added `render` method and some methods for changing the rendering engine (`Template`).

Using a `Component`, a library may return raw data, but with methods that make it display-ready. The application that receives this raw data may then display it, optionally changing the way it's displayed and/or further manipulating it before display. Since `Components` can contain nested `Components`, it's possible for an application that knows what type of `Component` it's dealing with to traverse the tree, accessing many levels of data and changing the way each sub-`Component` is displayed. For example, an application may descend an entire `Component` tree and set each `Component`'s template itself, effectively changing everything about the way a final `Component` appears on screen, without having to have built the `Component` itself.

This allows us to freely pass `Components` as the return value of library methods, thus ensuring that our libraries are and remain compatible with any application.

(Note that `Component` is little more than a specialized array. One could argue that simply passing arrays of data around would be equally useful, though I contend that data is very naturally associated with a method for output and so it makes sense not only to keep these two elements bound together in a single object, but also to define them in a tree form that allows us to render all data into a display with a single call.)

**Best Practice:** Pass `Component` objects around whenever possible. Give components useful default `Templates` so that they display out of the box instead of throwing exceptions. When creating data objects, whether as part of an object-relational database or as independent objects, implement `Component` so that these may be used effortlessly by other libraries and apps. Use `ComponentCollection` for arrays of `Components`.

#### Data Abstraction {#datalayer}

Skel was built to assume the use of a database abstraction layer. Rather than mixing SQL into your library, your library should instead utilize an object that implements the `Db` interface or (more probably) your derivative of it. There's actually very little that the native `Db` interface does out of the box, and this is by design. Since data is highly app-specific, only minimal assumptions can be made about how to query data (with the exception of more cut-and-dry object-relational database systems). Thus, it is assumed that your implementation of `Db` -- be it a custom implementation or an extension of the official Skel implementation -- adds a number of very specific data access methods that your app and libraries require. (The `Db` implementation provided in the [skelphp-db package](https://github.com/kael-shipman/skelphp-db) assumes the use of a PDO data back end, and also incorporates some useful concepts from the Android Application Framework for schema updates.)

One of the reasons for the very loose and undefined handling of data in Skel is that I find I often want to store various types of data differently. For example, I may want to define my menu structure in YAML, or I may want to store blog posts as text files. Using a declarative database abstraction layer allows me to store the data however I want (and, if I define a data interface for my app, it allows others to replicate my app, but store the data differently). As it turns out, this is not a wildly important detail, but it's one that's very significant in terms of how we relate to the data we use.

**Best Practice:** Anything that looks like data should be accessed through your `Db` object or derivative, interfaced with a very specific, declarative method. There should never be SQL in your library or app, and you should also be careful about using things like `file_get_contents`, since this also suggests a call for data. The very simple menu structure for my personal website, for example, is stored as php arrays within a function called `getMenuItems` in my personal extension of the Skel `Db` class. `getMenuItems` accepts a string `$menuName` and returns an array that represents the options with names and urls. If someday I move this to the database, I don't have to change my app -- I just have to change my Db class to look for the menu items in a different place.

#### Interface, Interface, Interface

The idea of the OOP interface was a totally crucial detail that appears to have been yet another casualty of the emergence of loosely-typed web applications languages like PHP. I admit, I was ignorant of its benefits until my forays into Java in 2016. There's no time like the present, though, and it's my strong opinion that all method arguments should be type-hinted with either primative types of interfaces. What this means is that, barring only the simplest of apps, you should have your own Interfaces header file (just like the Skel Header package) that defines your interfaces and exceptions. That way, if someone wants to use your work, they can use it in the same way they use the Skel framework itself -- which is to say, they can choose to incorporate your implementations, or they can incorporate your header file and write their own implementations.

So do create interfaces, and when you write methods, type-hint using your interfaces -- not your classes.
