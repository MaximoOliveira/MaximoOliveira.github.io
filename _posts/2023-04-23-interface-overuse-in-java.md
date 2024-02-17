---
layout: post
title: "Eliminating One-to-One Interfaces in Java"
author: "Maximo Oliveira"
tags: Anti-pattern Interface-Overuse
excerpt_separator: <!--more-->
---

Unfortunately, it´s still common to see the practice of coupling every class with an interface, even if that means we only have one implementation.
<!--more-->
Many resources discuss this problem [[1](#ref-1), [2](#ref-2),
[3](#ref-3), [4](#ref-4), [5](#ref-5)], but they fail to offer practical guidance on how to refactor it in an existing project.

This post aims to address the issue by providing a quick refactoring solution.

# Single implementation interface

I repeatedly come across the following anti-pattern in Java projects:

{% highlight java %}
Service service = new ServiceImpl();
{% endhighlight %}

Essentially, each class follows a naming convention where it is named `_ServiceImpl` and is the only implementation of an interface named `_Service`.

Although interfaces have their place, creating one that only mirrors one class is counterproductive and leads to the following problems identified by Adam Bien 13 years ago [[3](#ref-3)]:


 > Imagine you get another implementation (thats the whole point of an interface) - how would you name it?

<div style="padding-left: 30px;">
If a suitable name cannot be found for an implementing class, it often indicates that the class may be overly complex or trying to do too many things, potentially violating the Single Responsibility principle.
</div>
<br>

> It doubles the amount of artifacts - and significantly increases the "complexity"

<div style="padding-left: 30px;">
Now you have the clutter of two files instead of one for everything.
</div>
<br>

> The navigation in the IDE is less fluent

<div style="padding-left: 30px;">
When you´re debugging and jump into a method call from ServiceImpl, you´re redirected to the interface signature and not the actual implementation you want. Although it may appear to be a minor inconvenience, it's important to note that as a software developer, the majority of your time is spent reading code.
</div>
<br>

For a more in-depth read on this problem, I recommended the first section of [Victor Rentea´s post](https://victorrentea.ro/blog/overengineering-in-onion-hexagonal-architectures/).

Now let´s see how we can quickly refactor this anti-pattern when it occurs frequently within a project.

# Refactor Unnecessary Interfaces

1. Press Shift twice to bring up the `Search Everywhere` dialog.
2. Type `run inspection by name` and select the option when it appears.
![inspection](../assets/single-implementation-interface\step2.png "run inspection by name")
3. In the `Enter inspection name` dialog, type `interface with a single direct inheritor` and select it from the list.
![single direct inheritor](../assets/single-implementation-interface\step3.png "interface with a single direct inheritor")
4. Choose the desired inspection scope, such as `Project Files`, `Whole Project`, or a custom scope.
5. Click `OK` to run the inspection.
6. This will show you a list of all the interfaces in the selected scope that have only one implementation.
![one-implementation](../assets/single-implementation-interface\step6.png "interface that have only one implementation")
7. We are now going to [Inline](https://www.baeldung.com/intellij-refactoring#inlining) an interface by double clicking on one of results, click on the interface name and press `Ctrl+Alt+N` (or `Cmd+Option+N on macOS`) to bring up the "Inline" refactoring window and press Enter. You can also right click on the interface name -> Refactor -> Inline to Anonymous Class.
![inline-references](../assets/single-implementation-interface\step7.png "Inline all references")

With these steps all references to a single interface are removed and replaced with the class implementation. However we are still left with the `Impl` suffix in the class names. To get rid of this we will use the Find and Replace functionality:

1. Press `Ctrl+Shift+R` (or `Cmd+Option+R on macOS`) to open the Replace in Path dialog. Alternatively right click on project and click `Replace in Files...`

2. In the top field, enter `Impl` and make sure you have the `Cc` button enabled. Leave the bottom field empty:
![replace-all](../assets/single-implementation-interface\step2.2.png "Replace all")

3. Finally click on `Replace All`.


## References

1. <a id="ref-1"></a>Martin Fowler. "InterfaceImplementationPair." Martin Fowler's bliki, 2014. <https://martinfowler.com/bliki/InterfaceImplementationPair.html> 


2. <a id="ref-2"></a>Alexsandro Souza . "Best Practices in Software Development: Interface Overuse" DZone, 2020. <https://dzone.com/articles/good-practices-interface-overuse> 

3. <a id="ref-3"></a>Adam Bien. "Service s = new ServiceImpl() - Why?" Adam Bien's Weblog, 2010. <https://www.adam-bien.com/roller/abien/entry/service_s_new_serviceimpl_why>

4. <a id="ref-4"></a>Anthony Steele. "Interfaces are overused" Anthony Steele's Weblog, 2021. <https://www.anthonysteele.co.uk/InterfacesAreOverused>

5. <a id="ref-4"></a>Victor Rentea. "overengineering-in-onion-hexagonal-architectures" Victor Rentea's Weblog, 2023. <https://victorrentea.ro/blog/overengineering-in-onion-hexagonal-architectures/>