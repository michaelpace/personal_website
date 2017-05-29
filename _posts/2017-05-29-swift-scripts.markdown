---
layout: post
title:  "Swift scripts"
date:   2017-05-29 01:00:00 -0500
tags: swift
---

When iOS projects reach a certain size, it can be useful to have scripts to do certain things like TODO, TODO, and TODO. Most projects I've worked in have scripts to accomplish these tasks and others written in a language like bash, Python, or Ruby, and they all do the job admirably.

Why?
====

So why use Swift to write these scripts?

* You're already writing your app in Swift. It has a familiar syntax, and libraries you already know well. You can use Foundation, Cocoa, as well as any CocoaPods or Carthage packages you're familiar with.
* You can use Xcode.

A few things to note
====================

Before we get into the "how," it might be a good idea to note how code in your Swift script will differ from the code you write for an iOS or Mac app. Because you're writing code outside of the context of such an application, you won't have a run loop or an autorelease pool. TODO: Explain why this matters (lack of autorelease pools probably don't).

How?
====

Command line
------------

It's quite simple! Just save a new file with a `.swift` extension and run it with `/usr/bin/swift`:

{% highlight bash %}
➜  cat helloworld.swift
{% endhighlight %}
{% highlight swift %}
print("Hello, world!")
{% endhighlight %}
{% highlight bash %}
➜  swift helloworld.swift
Hello, world!
{% endhighlight %}

What if you wanted to pass in arguments? That's also easy, thanks to the `CommandLine` enum of Swift's standard library ([link](https://developer.apple.com/reference/swift/commandline)):

{% highlight bash %}
➜  cat hello.swift
{% endhighlight %}
{% highlight swift %}
let name = CommandLine.argc > 1 ? CommandLine.arguments[1] : "world"
print("Hello, \(name)!")
{% endhighlight %}
{% highlight bash %}
➜  swift hello.swift
Hello, world!

➜  swift hello.swift Michael
Hello, Michael!
{% endhighlight %}

Wonderful! But this is a script, right? Why don't we make it an executable? TODO: why would you want to do this?

{% highlight bash %}
➜  chmod +x hello.swift
{% endhighlight %}

We'll also have to add a line to the top of the script to instruct our shell how to run the script:

{% highlight swift %}
#!/usr/bin/swift

let name = CommandLine.argc > 1 ? CommandLine.arguments[1] : "world"
print("Hello, \(name)!")
{% endhighlight %}

Great!

{% highlight bash %}
➜  ./hello.swift Michael
Hello, Michael!
{% endhighlight %}

We are getting extremely fancy. We could get even fancier and compile the thing if we wanted: TODO: why would you want to do this?

{% highlight bash %}
➜  swiftc hello.swift 

➜  ./hello Michael
Hello, Michael!

{% endhighlight %}

Xcode
-----

We could have done all of the above in Xcode too, by starting a new project and choosing "Command Line Tool":

<a href="/images/swift-scripts/1.png"><img src="/images/swift-scripts/1.png"/></a>
<a href="/images/swift-scripts/2.png"><img src="/images/swift-scripts/2.png"/></a>
<a href="/images/swift-scripts/3.png"><img src="/images/swift-scripts/3.png"/></a>

This being Xcode, we can just click the "play" button to run the script, and our output will appear in the debug area:

<a href="/images/swift-scripts/4.png"><img src="/images/swift-scripts/4.png"/></a>
<a href="/images/swift-scripts/5.png"><img src="/images/swift-scripts/5.png"/></a>

We could also accept arguments as we did above. First, let's paste in the code we'd written before:

<a href="/images/swift-scripts/6.png"><img src="/images/swift-scripts/6.png"/></a>

Passing in arguments is as simple as modifying the target's scheme:

<a href="/images/swift-scripts/7.png"><img src="/images/swift-scripts/7.png"/></a>
<a href="/images/swift-scripts/8.png"><img src="/images/swift-scripts/8.png"/></a>
<a href="/images/swift-scripts/8.5.png"><img src="/images/swift-scripts/8.5.png"/></a>
<a href="/images/swift-scripts/9.png"><img src="/images/swift-scripts/9.png"/></a>

Executing command-line programs from your script
------------------------------------------------

A common task in scripts is to call other scripts or programs. How would you do that in Swift? We can use `Process` ([link](https://developer.apple.com/reference/foundation/process)):

{% highlight bash %}
➜  cat echo.swift             
{% endhighlight %}
{% highlight swift %}
#!/usr/bin/swift

import Foundation

let process = Process()
process.launchPath = "/bin/echo"
process.arguments = [CommandLine.arguments[1]]
process.launch()
{% endhighlight %}
{% highlight bash %}
➜  ./echo.swift "wuddup world"
wuddup world
{% endhighlight %}

In / out
--------

TODO: talk about freakin i/o from http://crunchybagel.com/building-command-line-tools-with-swift/

A note on dependencies
----------------------

This is all well and good, but many tasks you might want to write are more complex, and require third-party libraries. How might we use [Alamofire](https://github.com/Alamofire/Alamofire), for instance? We've got a few options:

* [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
* [Carthage](https://github.com/Carthage/Carthage)
* [cocoapods-rome](https://github.com/CocoaPods/Rome)

Let's go over these in order.

**[git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)**

TODO

**Carthage**

TODO

**cocoapods-rome**

TODO

Concurrency
-----------

TODO. dispatch groups, nsoperations, etc. note that things will return right away.

Interactive scripts
-------------------

TODO: runloops, or just do what wenderlich says

Swift packages
--------------

TODO

{% highlight bash %}
➜  swift package init 
{% endhighlight %}

Third-party tools that may be helpful
=====================================

* [cato](https://github.com/neonichu/cato) - "Swift scripts made easy with automatic dependency and Swift version management."
* [ShellOut](https://github.com/JohnSundell/ShellOut) - "Easily run shell commands from a Swift script or command line tool"