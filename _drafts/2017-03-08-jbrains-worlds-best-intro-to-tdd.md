---
layout: post
title: Jbrains' World's Best Intro To TDD
author: Douglas Waugh
excerpt: I recently completed Jbrains' World's Best Intro To TDD.  I decided to write this blog post to collect my thoughts.
---

Introduction
---

### My experience with TDD before the course.

Notes on what I would do differently if I had my time over again
---

I didn't follow along with the examples excatly at the beginning.  I put test doubles in immediately because it looked like the right thing to do.

Take Aways
---

These are the different things I've been thinking more about after completing the course.  I completed the list (mostly) without reference back to the material, so it represents the things that I really have remembered from the course.  Not all of these things are new to me but Jbrains has definitely made me think harder, and sometimes differently, about them.

### Moving details up and abstractions down

Jbrains suggests that as you build a system your tests should become more specific and your application should become more generic.  I'm not sure how I feel about this right now.  I think it could be big, but I'm not sure I ever think about it.  Interestingly today my colleague Richard Nagle shared a link to a [post by Uncle Bob](http://blog.cleancoder.com/uncle-bob/2017/03/03/TDD-Harms-Architecture.html) who had noticed a similar phenomenon.

One situation Jbrains where uses the technique is to remove the duplication between the code and the tests when his tests for `SaleController` contain very similar product data as the `SalesController` itself; `SalesController` has a map of barcode to price and its tests have individual barcodes and prices.  Pushing the map up to the tests by passing it in to `SaleController` as a dependency makes the `SaleController` more generic and puts the data that the tests rely on in setup, passing as parameters and in assertions close to each other, making the link between the three much clearer.

Uncle Bob has a similar reason for making his code more generic and his tests more specific and that is to avoid the test logic from duplicating the tests.  The post linked to above has a good example of this.

### Sensitivity to duplication

Most developers know that duplication in your code base is generally I bad idea which can be removed by finding a suitable abstraction.  Perhaps the strongest signal towards a missing abstraction is that given by code that has been liberaly copied and pasted throughout the code base.  However, there are different, more subtle forms of duplication, which Jbrains picks up on.

One is the duplication of the word 'display' in three different methods (`displayProductNotFoundMessage`, `displayEmptyBarcodeMessage`, `displayPrice`) and their containing class `ConsoleDisplay` (Series 5, Episode 8: A Long Look Down The Road).  This might be a signal that some of the work the method does could be extracted to a shared method with the argument the method receives varying the behaviour.  For more on this subject check out my blog on [Duplication and Abstraction](http://tech.energyhelpline.com/duplication-and-abstraction/).

### Sensitivity to levels of abstraction

Together with being sensitive to duplication within the code, being sensitive to different levels of abstraction is important feed back that the code gives you that can help you improve your design.  If a lower level abstraction has leaked in to a class it is likely to know too much about an implementation detail it doesn't need to care about.  Mixing different levels of abstraction within the same class is a bad idea because code at different levels of abstraction are likely to change at different rates and for different reasons.

The example that Jbrains has is within a class primarily concerned with the `Display` of `Messages`, he has a `PostOffice` which has a relevnace at a lower, in this case `UDP` level.  This is an indication that at the very least the naming is wrong, and perhaps there are other problems with the design.

### Lowest coupling

I've always liked the idea of using the tell-don't-ask principle I first read about in Nat Pryce and Steve Freeman's [Growing Object Oriented Software Guided By Tests](https://www.amazon.co.uk/Growing-Object-Oriented-Software-Guided-Signature/dp/0321503627) when designing interactions between objects.  Jbrains makes it clear why this is such a good idea; a method on an interface not expected to return anything is the weakest form of contract a method can have as, so long as the parameters passed remain the same, the implementing class can change as many times and it likes without affecting those things dependent on the interface.

### When to bend the rules

Despite what I've said in Low coupling, what I've always struggled with is how this fits in to a normal web application for which a response is required for every request, mandating that at least the controllers return something.  

Jbrains discusses this problem when attempting to remove the reference to `Display`, a concern of the application, from `SalesController`, a member of the domain in [Domain Driven Design](https://www.amazon.co.uk/Domain-driven-Design-Tackling-Complexity-Software/dp/0321125215) parlance.  He doesn't like the idea of losing the purity the design had when `SalesController` could just tell `Display` to render a message, but by `SalesController` returning an object that represents the message to be sent back up the call stack he is able to remove the reference to `Display` from the domain code.  As with many things in software engineering the decision is a trade off between two contradictory ideas both with merits and short-comings.

In this case Jbrains decides that, on balance, it's better to remove the reference to the application code from the domain code than to retain the tell-don't-ask design.  I can imagine that you might often have to do a bit of fudging when your beautiful snowflake domain code meets the horrible reality of application code.

### Collection test cases

Jbrains reminded me of the minimum number of test cases required to test a collection: none, one, some, many, and error.  A small point but nice to be reminded.

### Using object references for object equality

Usually when you assert that one object is the same as another you override the equality members to compare all the members, in the case of a value object, or the ID, in the case of an entity.  However, Jbrains shows that you can delay implementing the equality overides when you have a reference to the object you are asserting on in the test, i.e., you pass the object you later assert on in to the object under test, perhaps when checking the referenced object is passed to a dependency.

### git commits

TO begin with I really didn't like the vague commit messages that Jbrains uses such as 'extracted a method' or 'renamed a variable'.  However, over time, I have got used to them and now use similar commit messages.

I was already in favour of very tight commits, with anything not relevant to this particular change being committed separately.  I would even exclude removing an extraneous empty line or removing some unused namespaces as deserving of a different commit.  The less I have in a commit that isn't truely about the change I am committing, the easier it is for me to see what the change was when I come back to it.

Favourite episodes
---

### Series 4, 8: A long look down the road

An indepth refactoring of a small class.  I think this episode gives a good insight to the level of sensitivity to duplication, naming, and levels of abstraction required of a developer.

### Series 5, Episode 6: Before we move on

Jbrains walks through a number of smells he can see in the code and gives possible solutions to them.  Again it's nice to see what design issues catch his attention, why they catch his attention and what he would like to do about them.

Conclusion
---

Definitely recommend the course to TDD beginners and intermediates (can't speak for the more advanced).

I'm not sure if all the worts were added, but enough were to make it feel as though I was watching somebody discover a design, with a running commentary to understand the developer's thought process.  Using talking about the problem to help concepts coelesce in our minds.