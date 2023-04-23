---
layout: post
title: "Interface Overuse in Java"
author: "Maximo Oliveira"
tags: Anti-pattern
---

# Single implementation interface

I repeatedly come across the following anti-pattern in Java projects:

Service service = new ServiceImpl();

Essentially, each class follows a naming convention where it is named "_ServiceImpl" and is the only implementation of an interface named "_Service".


Although interfaces have their place, creating one that only mirrors one class implementation is often an unnecessary step as it leads to the following problems [identified by Adam Bien](https://www.adam-bien.com/roller/abien/entry/service_s_new_serviceimpl_why) 13 years ago:


**1. Imagine you get another implementation (thats the whole point of an interface) - how would you name it?**

<div style="padding-left: 30px;">
If a suitable name cannot be found for an implementing class, it often indicates that the class may be overly complex or trying to do too many things, potentially violating the Single Responsibility principle.
</div>
<br>

**2. It doubles the amount of artifacts - and significantly increases the "complexity"**
<div style="padding-left: 30px;">
Now you have the clutter of two files instead of one for everything - 1 for the price of 2.
</div>
<br>

**3. The navigation in the IDE is less fluent**

<div style="padding-left: 30px;">
When you´re debugging and jump into a metod call from _SerivceImpl, you´re redirected to the interface signature and not the actual implementation you want. Although it may appear to be a minor inconvenience, it's important to note that as a software developer, the majority of your time is spent reading code.
</div>
<br>


# Fix this shit

If you find yourself working on a project that follows this anti-pattern
you can easily refactor this using InteliJ by following these steps:

1. Press Shift twice to bring up the 'Search Everywhere' dialog.
2. Type "run inspection by name" and select the option when it appears.
![inspection](../assets/single-implementation-interface\step2.png "run inspection by name")
3. In the 'Enter inspection name' dialog, type "interface with a single direct inheritor" and select it from the list.
![single direct inheritor](../assets/single-implementation-interface\step3.png "interface with a single direct inheritor")
4. Choose the desired inspection scope, such as 'Project Files', 'Whole Project', or a custom scope.
5. Click 'OK' to run the inspection. 
6. This will show you a list of all the interfaces in the selected scope that have only one implementation.
![one-implementation](../assets/single-implementation-interface\step6.png "interface that have only one implementation")
7. Double click on one of results, click on the class name and press Ctrl+Alt+N (or Cmd+Option+N on macOS) to bring up the "Inline" refactoring window and press Enter. You can also right click on the class name -> Refactor -> Inline to Anonymous Class.
![inline-references](../assets/single-implementation-interface\step7.png "Inline all references")

With these steps all references to a single interface are removed and replaced with the class implementation. However we are still left with the Impl suffix in the class names. To get rid of this we can use [Structural Search and Replace](https://www.jetbrains.com/help/idea/structural-search-and-replace.html) to quickly find all Java classes that end with the suffix "Impl" and remove this suffix:

1. From the main menu, select Edit | Find | Replace Structurally.

2. In the Search template field, enter the following code:

See also:

* [InterfaceImplementationPair](https://martinfowler.com/bliki/InterfaceImplementationPair.html)
