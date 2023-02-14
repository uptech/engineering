+++
title = "Inversion of Control"
description = "An exploration of Inversion of Control and I think about it and I how I identify control flow code and Inversion of Control opportunities."
date=2023-02-09
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/ts-logo.png"
canonical = "https://drewdeponte.com/blog/inversion-of-control/"
+++

Within the last month, I have interacted with several developers that don't have
a great understanding of the concept of *Inversion of Control (IoC)*. Just the
other day one of the developers at [Uptech Studio][] asked me to explain it
after I mentioned it as a way of separating DB concerns from business logic
concerns when the DB concerns are tied together by a DB transaction.

I introduced this developer to the concept of *IoC* and walked him through how
I think about code and the concept of *Separation of Concerns* in a way that
enables me to more easily understand how and where to apply *IoC*. So I figured
I might as well write it up so that I can share it with the rest of the world
in hopes that it can help others as well.

So let's get into it.

## What Is Inversion of Control?

The other day I was talking to another developer about struggling to find a
good resource that really explained *IoC*, and half-jokingly he asked
ChatGPT to explain it with the following.

> Can you explain the coding principle “inversion of control”?

It came back with the following.

> Inversion of control (IoC) is a software design principle that encourages the
> design of loosely coupled components in which the flow of control is not
> determined by a component itself, but by a container or framework that is
> responsible for assembling the components and coordinating their behavior.
> This results in a separation of concerns between the components and the
> container, where the components focus on their own specific tasks, while the
> container is responsible for managing their interactions.
>
> IoC is often achieved through dependency injection, in which the container
> supplies the components with their dependencies, rather than the components
> creating or looking up their own dependencies. This leads to a flexible and
> modular architecture, where components can be easily replaced or reused
> without affecting the rest of the system.

To be honest it did a great job at returning a pretty clear definition of what
*IoC* is. Good enough in fact that I wouldn't even try to change it. Trying to
get ChatGPT to provide meaningful examples in article form, that I could hand
someone to teach them *IoC*, was a massive failure. And as we all know being able
to read a conceptual definition like the one above and being able to understand
it, internalize it, and apply it while coding in the wild is an
entirely different thing. Hence, why I am writing this article.

## IoC at all the levels

*IoC* as a concept is applied & can be applied across all the different levels
of abstraction in programming. At the higher levels, we can classically see it
with the distinction between the concept of libraries and frameworks.
Frameworks are simply libraries that apply the concept of *IoC*.

If we consider how we use a GUI framework. We generally create some sort of
Application level object and call `run()` on it. The GUI framework is in
control of the flow of execution of the program and reaches out to other
objects, we provide, to ask them to perform different pieces and hand the
framework back results so that it can continue to manage the control flow of
the application. On the other hand, if we were using a GUI library. We would be
having to manage the control flow of the application ourselves, and we would in
turn be calling out to different functions/methods in the library to do
different pieces for us. So with this distinction, we can see that in the
library case we are responsible for the control flow of the application but in
the framework case we hand that responsibility of control flow over to the
framework.

At the lower levels of abstraction, we classically see it with most higher-order
functions, especially the higher-order functions on collections, e.g. `map`,
`reduce`, `filter`, etc. If we consider using `filter` as follows.

```typescript
someCollection.filter((item) => item.isOld)
```

We are handing the control flow of the program over to the `filter()` procedure
while it asks us to provide criteria to filter with via the provided closure.
This is an example of *IoC* just as the framework vs. library concept is. The
big distinction between them is that in the case of a full application
framework you are handing over control for the lifetime of the application. In
the case of `filter()` we are handing over control only for the lifetime of
that particular application of the filter process.

The key is understanding that we can hand over the control flow of the
application at any of the levels of abstraction and it is all *IoC*. It is
inverting the control by handing it over to something that in classical
imperative procedural code does not have the "control".

## Understanding IoC with Code

To better understand how to think about this concept in relation to code let's
take a look at a simple example where we have a collection of numbers and
we want to have a new collection of numbers that has each value in it but with
5 added to it. In classic imperative procedural code, we would implement this
something like the following.

```typescript
const myNumbers: Array<number> = [2, 4, 7, 13]
let myNewNumbers: Array<number> = []

for (num in myNumbers) {
	let newNum = num + 5
	myNewNumbers.push(newNum)
}

// myNewNumbers is now [7, 9, 12, 18]
```

### Separation of Concerns

Before we can start to think about the possibility of applying the concept of
*IoC* we have some groundwork that we have to do first. We need to understand
the various *concerns/responsibilities* involved in the code and classify the
various lines, statements, and expressions in terms of these
*concerns/responsibilities*. It is also extremely useful to understand the
high-level goal of the segment of code being analyzed as it will help form our
thinking conceptually when applying *IoC*.

It is probably worth talking about the idea of a *concern*, *responsibility*,
*domain*, or *problem space* which are mostly used interchangeably. They all are
used to define a particular bound around a problem. The difficulty people find
with them is that you can in theory take a bound around any problem and break
it down further into smaller bounds around sub-problems infinitely. The other
thing that people tend to find difficult is that they are arbitrary. Meaning
you can define the bounds to be whatever you want. In practice in software
development, this is simply defining a bound around a problem space and
generally mapping an abstraction of some sort to that bound.

This breaking up of a bound around a problem into sub-problems with their
bounds is the foundation of the concept of *Separation of Concerns*.
And as we will see throughout this process *IoC* is simply a specific example
of *Separation of Concerns*.

### Understanding the Concerns

Let's take a look at the example from above and see if we can identify the
overall *concern/responsibility* and then try to classify concerns that the
various lines of code belong to.

Overall the *concern* of the example above is to take the `myNumbers` array and
create a new array containing the values from `myNumbers` but with 5 added to
it. That doesn't feel like much of a *concern*, but technically it is and since
it is what we have right now let's run with it.

Now that we feel like we have a high-level *concern* figured
out let's think about *IoC* a bit deeper. We talked at a high level about what
the inversion portion of *IoC* is. But what is the control portion in our current
context. The way I think about it is by trying to break the overall *concern*
into *sub concerns* where one of the *sub concerns* is the "control".

To do that we need to understand what the "control" is. It is essentially any
portion of the code that controls the flow of a process. So we have to start
thinking about the code that controls the flow of a process as one *concern*
and the various steps/pieces of the process as other *concerns*. I also think
about this as code representing one of two things, the triggering of doing
something & the actual how of doing something. So we can start thinking about
the ability to separate the how of doing something from the triggering of doing
something. It may not be a perfect way of thinking, but it has tended to help
me.

Let's try and define the various concerns of our example. It is important to
realize that this is a process. It isn't magical, and you don't always know all
the *concerns* right out of the gate. Sometimes you just start knowing that
one concern is the "control" and the others you aren't sure about yet. Removing
the noise of the code that you know is the "control" can help you think more
clearly about the code that is **not** control and its respective *concerns*.
Below is our example from above in classical imperative procedural form. We
will annotate it with comments to try and communicate the concerns as we
progress below.

```typescript
const myNumbers: Array<number> = [2, 4, 7, 13]
let myNewNumbers: Array<number> = []

for (num in myNumbers) {
	let newNum = num + 5
	myNewNumbers.push(newNum)
}

// myNewNumbers is now [7, 9, 12, 18]
```

Looking at `const myNumbers: Array<number> = [2, 4, 7, 13]` it is just
providing the starting value for the process. It isn't controlling the flow of
the process. Therefore, I would **not** classify it as part of the "control"
*concern*. So let's tag it with an empty comment `//`.

```typescript
const myNumbers: Array<number> = [2, 4, 7, 13]  // other
let myNewNumbers: Array<number> = []

for (num in myNumbers) {
	let newNum = num + 5
	myNewNumbers.push(newNum)
}

// myNewNumbers is now [7, 9, 12, 18]
```

If we look at the `let myNewNumbers: Array<number> = []` on the first line of
code and ask ourselves "Why is it there?" In this case, it is there to
support the flow of this process by being the new array that the new values
will live in. So let's flag it as "control" with a comment.

```typescript
const myNumbers: Array<number> = [2, 4, 7, 13]  //
let myNewNumbers: Array<number> = []            // control

for (num in myNumbers) {
	let newNum = num + 5
	myNewNumbers.push(newNum)
}

// myNewNumbers is now [7, 9, 12, 18]
```

OK so what about the `for (num in myNumbers) {`? Well, it is there to
facilitate iterating through the collection. This sounds like
controlling the flow of the process to me. So let's flag it as control.

```typescript
const myNumbers: Array<number> = [2, 4, 7, 13]  //
let myNewNumbers: Array<number> = []            // control

for (num in myNumbers) {                        // control
	let newNum = num + 5
	myNewNumbers.push(newNum)
}                                               // control

// myNewNumbers is now [7, 9, 12, 18]
```

OK, now we are onto `let newNum = num + 5`. This is interesting because this is
doing two things. The `let newNum =` portion is controlling the flow of the
process while the `num + 5` portion is more about the work of computing a new
value and not really about the flow control of the process. So let's flag it to
specify this detail.

```typescript
const myNumbers: Array<number> = [2, 4, 7, 13]  //
let myNewNumbers: Array<number> = []            // control

for (num in myNumbers) {                        // control
	let newNum = num + 5                        // `let newNum =` control, `num + 5` some other concern
	myNewNumbers.push(newNum)
}                                               // control

// myNewNumbers is now [7, 9, 12, 18]
```

What about `myNewNumbers.push(newNum)`? This seems to be code that manages how
this process flows, so it seems to be control. Let's flag it as control.

```typescript
const myNumbers: Array<number> = [2, 4, 7, 13]  //
let myNewNumbers: Array<number> = []            // control

for (num in myNumbers) {                        // control
	let newNum = num + 5                        // `let newNum =` control, `num + 5` some other concern
	myNewNumbers.push(newNum)                   // control
}                                               // control

// myNewNumbers is now [7, 9, 12, 18]
```

At this point, we have classified all lines, statements, and expressions of code,
and it feels like we have a pretty decent understanding of what code exists for
the purpose of controlling the flow of the process and which code doesn't. In
fact there are only two chunks of code that don't belong to the "control"
*concern*, `const myNumbers: Array<number> = [2, 4, 7, 13]` and `num + 5`.

### Grouping & Separating Concerns

Now that we have an understanding of at least some portion of the various
concerns. We need to focus on conceptually grouping and separating them. In
our case, we have the "control" *concern* and then two pieces of code that right
now we know are not part of the "control" *concern*. The first step in
separating concerns is to simply map those concerns to specific abstractions.

To start let's take all the code that is "control" code and extract it to a
function we will call `foo` for now. *Note:* I use a garbage name for the
function like `foo` right now because I like to think about the concern
conceptually after having moved the "control" code into it as I think it makes
it easier to think about. If we do this we end up with something like the
following.

```typescript
function foo() {
    let myNewNumbers: Array<number> = []

    for (num in _________) {
	    let newNum = _______
	    myNewNumbers.push(newNum)
    }
}

const myNumbers: Array<number> = [2, 4, 7, 13]

	num + 5

// myNewNumbers is now [7, 9, 12, 18]
```

I put `________` in the function where the other *concerns* were interacting
before. This helps us think about those elements in a more generic way.
To make this function usable we have to fill in those `______` blanks. The only
way to do that while maintaining the abstraction is to pass them in as
parameters to the function. Our example would now look as follows.

```typescript
function foo(myNumbers: Array<number>, op: (number) => number) {
    let myNewNumbers: Array<number> = []

    for (num in myNumbers) {
	    let newNum = op(num)
	    myNewNumbers.push(newNum)
    }

	return myNewNumbers
}

foo([2, 4, 7, 13], (num) => num + 5) // => [7, 9, 12, 18]
```

By doing this we have not only made the `foo` function usable. But we have made
it more generic as we can now hand it any Array of numbers to operate on, and we
can hand it any operation to perform on each element as long as it results in a
number.

At this point, we have successfully **inverted control**, and we have done so by
identifying the control code, extracted it out into a function, and then using
that function in its original place. It is important to understand that this
is **IoC** because when we call the `foo()` function. We are handing control
over to the `foo()` function for the lifetime of the `foo()` process while we
are still providing the details of the work it does.

### Generalize Types

When using IoC it is common practice to generalize functions to remove
unnecessary coupling to specific types from the "control" code. This is another
mechanism for focusing only on the flow control of the process and less on the
specifics of the types involved. This also has the benefit of generally making
the extracted abstraction more flexible.

If we start by looking at `myNumbers: Array<number>` and that it is only used
in the `for (num in myNumbers)` line. `for (x in y)` doesn't care what type of
array `y` is. It just cares that it is an Array. If we make that change it
would look as follows.

```typescript
function foo<A>(myNumbers: Array<A>, op: (number) => number) {
    let myNewNumbers: Array<number> = []

    for (num in myNumbers) {
	    let newNum = op(num)
	    myNewNumbers.push(newNum)
    }

	return myNewNumbers
}

foo([2, 4, 7, 13], (num) => num + 5) // => [7, 9, 12, 18]
```

This creates a temporary problem which is that the typing of `op` would now
also need to change so that it would take in a `A`. And then we are immediately
confronted with the next question. What type does `op` return. From looking at
the code, currently we have the values ending up in `myNewNumbers` which is a
`Array<number>`. So we would have to have it return a `number` if we didn't
change the code further. But it doesn't seem like there is any reason we
shouldn't just allow that array to be generic as well so that `op` can return a
different type. This would allow the user to convert the types of
elements within a collection if they wanted. We should also update the names of
the parameters to represent more generic concepts.

```typescript
function foo<A, B>(collection: Array<A>, op: (A) => B) {
    let newCollection: Array<B> = []

    for (item in collection) {
	    let newItem = op(item)
	    newCollection.push(newItem)
    }

	return newCollection
}

foo([2, 4, 7, 13], (num) => num + 5) // => [7, 9, 12, 18]
```

The above `foo()` function now supports our existing use case of wanting to add
5 to each element of the array but also supports a bunch of other cool things
we can do because we have simply focused on separating an abstraction that
focuses as much as possible on the control flow of the process. This is what
*Inversion of Control* is all about.

### Naming Abstraction

So now it seems we need to think about what our `foo()` function
facilitates as a process, and we should try and come up with a meaningful name.
It lets us map each element of a collection from one value into
another value. So maybe we should call it something like `map()`.

```typescript
function map<A, B>(collection: Array<A>, op: (A) => B) {
    let newCollection: Array<B> = []

    for (item in collection) {
	    let newItem = op(item)
	    newCollection.push(newItem)
    }

	return newCollection
}

map([2, 4, 7, 13], (num) => num + 5) // => [7, 9, 12, 18]
```

This is actually how the standard `map()` function is implemented.

## Conclusion

We still have the "other" concern that we talked about before. What is
interesting is that through this process that concern has also received its own
abstraction, the `op` closure.

So now we can see fully how Inversion of Control is a specific type of
Separation of Concerns, as the "control" concern and "map item value" concern
have been separated by abstractions. We can also see how the standard `map`,
`reduce`, and `filter` functions are examples of IoC. But we have also seen
what the process looks like for classifying and separating concerns looks like,
which is a great skill that is applicable far beyond Inversion of Control.

[Uptech Studio]: https://uptechstudio.com
