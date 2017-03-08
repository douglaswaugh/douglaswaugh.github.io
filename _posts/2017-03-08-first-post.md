---
layout: post
title: Jbrains' World's Best Intro To TDD
author: Douglas Waugh
excerpt: I recently completed Jbrains' World's Best Intro To TDD.  I decided to write this blog post to collect my thoughts.
---

Introduction
---

#### My experience with TDD before the course.

Notes on what I would do differently if I had my time over again
---

I didn't follow along with the examples excatly at the beginning.  I put test doubles in immediately because it looked like the right thing to do.

Middle bit
---

These are the different things I've been thinking more about after completing the course.  I completed the list (mostly) without reference back to the material, so it represents the things that I really have remembered from the course.  Not all of these things are new to me but Jbrains has definitely made me think harder, and sometimes differently, about them.

#### Moving details up and abstractions down

Jbrains suggests that as you build a system your tests should become more specific and your application should become more generic.  I'm not sure how I feel about this right now.  I think it could be big, but I'm not sure I ever think about it.  Interestingly today my colleague Richard Nagle shared a link to a [post by Uncle Bob](http://blog.cleancoder.com/uncle-bob/2017/03/03/TDD-Harms-Architecture.html) who had noticed a similar phenomenon.

One situation Jbrains where uses the technique is to remove the duplication between the code and the tests when his tests for `SaleController` contain very similar product data as the `SalesController` itself; `SalesController` has a map of barcode to price and its tests have individual barcodes and prices.  Pushing the map up to the tests by passing it in to `SaleController` as a dependency makes the `SaleController` more generic and puts the data that the tests rely on in setup, passing as parameters and in assertions close to each other, making the link between the three much clearer.

Uncle Bob has a similar reason for making his code more generic and his tests more specific and that is to avoid the test logic from duplicating the tests.  The post linked to above has a good example of this.

#### Sensitivity to duplication

Most developers know that duplication in your code base is generally I bad idea which can be removed by finding a suitable abstraction.  Perhaps the strongest signal towards a missing abstraction is that given by code that has been liberaly copied and pasted throughout the code base.  However, there are different, more subtle forms of duplication, which Jbrains picks up on.  

One is the duplication of the word 'display' in three different methods (`displayProductNotFoundMessage`, `displayEmptyBarcodeMessage`, `displayPrice`) and their containing class `ConsoleDisplay` (Series 5, Episode 8: A Long Look Down The Road).  This might be a signal that some of the work the method does could be extracted to a shared method with the argument the method receives varying the behaviour.  For more on this subject check out my blog on [Duplication and Abstraction](http://tech.energyhelpline.com/duplication-and-abstraction/).

#### Sensitivity to levels of abstraction

Together with being sensitive to duplication within the code, being sensitive to different levels of abstraction is important feed back that the code gives you that can help you improve your design.  If a lower level abstraction has leaked in to a class it is likely to know too much about an implementation detail it doesn't need to care about.  Mixing different levels of abstraction within the same class is a bad idea because code at different levels of abstraction are likely to change at different rates and for different reasons.

The example that Jbrains has is within a class primarily concerned with the `Display` of `Messages`, he has a `PostOffice` which has a relevnace at a lower, in this case `UDP` level.  This is an indication that at the very least the naming is wrong, and perhaps there are other problems with the design.

#### When to bend the rules

example returning from the controller to remove the application code (in this case `Display`) from the domain code.

#### Low coupling

Despite subverting the rule in the end, it is interesting how a method without a return value is the weakest form of contract; the calling class has no interest in the implementation of the interface, only that its dependencey implements it.

#### Collection test cases

Collection test cases: None, one, some, many, error.

#### Using object references for object equality

Using object reference equality to delay implementing equality overrides

#### git commit messages

They way Jbrains structures his commit messages has stayed with me a bit, despite hating them to begin with.

Favourite episodes
---

Series 4, 8: A long look down the road
Series 5, Episode 6: Before we move on


Conclusion
---

Definitely recommend the course to TDD beginners and intermediates (can't speak for the more advanced).

I'm not sure if all the worts were added, but enough were to make it feel as though I was watching somebody discover a design, with a running commentary to understand the developer's thought process.  Using talking about the problem to help concepts coelesce in our minds.