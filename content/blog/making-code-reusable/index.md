+++
title = "Making Code Reusable"
description = "An exploration of how we can make code more reusable."
date=2023-01-18
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/ts-logo.png"
canonical = "https://drewdeponte.com/blog/making-code-reusable/"
+++

One thing that people don't seem to naturally understand is that to make code
reusable you have to make it generic. To get a better understanding of this
lets walk through some examples and see if we can get a better understanding of
some good ways to make code more generic.

Let's say we have a class called `User` and user has a `firstName` and
`lastName` properties. It might look something like the following.

```typescript
class User {
  constructor(public firstName: string, public lastName: string) { }
}
```

Now we find out that there is a need to access the user's `fullName` within the
application.

## Computed Property/Getter

Most people's natural instinct is to simply add a `fullName` getter to the
`User` class as a computed property.

```typescript
class User {
  constructor(public firstName: string, public lastName: string) { }

  get fullName(): string {
    return this.firstName + " " + this.lastName;
  }
}
```

This is great! Our immediate need is met. It lets us get the full name of a
user as follows.

```typescript
const user = new User('Bob', 'Villa');
const fullUserName = user.fullName;
```

However, this code isn't reusable. You need to ask yourself, "What does the
action of combining a first name and last name conceptually have to do with a
user?" The answer is, nothing other than that the user happens to have a first
name and a last name.

So how do we go about making this code more reusable? Well, step one is going
back to the version of `User` that didn't have a `fullName` getter.

```typescript
class User {
  constructor(public firstName: string, public lastName: string) { }
}
```

## Parameterized Function

Let's try pulling this `fullName` computed property out into a parameterized
function and see how that feels.

```typescript
function fullName(firstName: string, lastName: string): string {
  return firstName + " " + lastName;
}
```

Now we can get the full name as follows.

```typescript
const user = new User('Alice', 'Jones');
const fullUserName = fullName(user.firstName, user.lastName);
```

Ok now that we have extracted this into a function that is parameterized based
on the `firstName` and `lastName` arguments it is able to combine any pair of
`firstName` and `lastName` irrespective of where they came from. This is now
reusable by definition.

However, you may have noticed that the ergonomics is not quite as nice as it
was with the Getter/Method approach. Let's see if we can improve the ergonomics
a bit.

## Function Parameterized to Anonymous Interface

One of the complexities we have is that with it strictly parameterized to
`firstName` and `lastName` we have to create a mapping between the
`otherUser.firstName` and the `otherUser.lastName` by passing them as
parameters. It would be nice if we could simply pass a `User` to the `fullName`
function.

```typescript
function fullName(user: User): string {
  return user.firstName + " " + user.lastName;
}
```

This simply moves the knowledge of how to access the `firstName` and `lastName`
of the `User` object from outside the function to the inside the
function. This has a problem though. We just made the code no longer reusable,
because it once again only works with objects of type `User`.

To address these we can use TypeScript's anonymous interface typing to specify
that the function should accept any object that has both a property of
`firstName` and a property of `lastName`. This looks as follows.

```typescript
function fullName(named: { firstName: string, lastName: string }): string {
  return named.firstName + " " + named.lastName;
}
```

Which allows us to now get the full name of the user as follows.

```typescript
const user = new User('Bob', 'Villa');
const fullUserName = fullName(user);
```

Woot woot! We regained the generic and reusable nature that we had in the
parameterized function but in the cases where we have an object that matches
the properties of `firstName` and `lastName` it streamlines the usage. In the
cases where the properties don't match that we simply have to do a mapping
similar to the parameterized function example.

Another cool thing about this is that it is a function that takes a single
argument which now plays much nicer in a functional programming world.

One question people generally ask at this point is where do I put this
`fullName` function though. If it isn't part of `User` it shouldn't be in the
same file as `User`. I would argue you are correct. It has nothing to do with
`User`. This is where the concept of domain, a.k.a. problem space, a.k.a.
concern comes into play. Organizing code based on concern is a much more useful
strategy than by data as data is a byproduct of the concern. In this particular
case we have a concern of "Name Composition" so I would move it to a
TypeScript module named `name_composition.ts` for the time being.

You may have also noticed that we aren't 100% back to the ergonomics oh what we
had at the very beginning with the Getter/Method approach. We can gain the full
ergonomics back by adding the following getter to the `User` class as follows.

```typescript
class User {
  constructor(public firstName: string, public lastName: string) { }

  get fullName(): string {
    return fullName(this);
  }
}
```

This gives us the best of both worlds as it gives us the reusability of the
`fullName()` function while also giving us the convenience of having the
operation be discoverable via tooling on the `User` class. In the end giving us
the ability to get the `fullName` with either of the two following approaches.

```typescript
const user = new User('Bob', 'Villa');
const fullUserName = fullName(user);
const fullUserName2 = user.fullName;
```

I personally prefer not having the getter on the `User` class as it is just
another layer of abstraction that isn't necessary. However, a lot of people
value the discoverability of operations through tooling on the `User` class to
a high degree and think it is worth the cost.

## Function Parameterized to Named Interface

Another option we have is to parameterize the function based on a named
interface. For example, we could have an interfaced called `Named` which would
described our constraints of an object that has both a `firstName` and
`lastName` property.

```typescript
interface Named {
  firstName: string;
  lastName: string;
}
```

This would then allow us to write our function as follows.

```typescript
function fullName(named: Named): string {
  return named.firstName + " " + named.lastName;
}
```

This is definitely more readable and easier to conceptually understand that the
`fullName()` function operates on conceptually any named object. However, it
does add some more weight to the implementation. So I can see going back and
forth on this. I think if there is more than one function that operates on the
concept of a `Named` object then it should definitely be formalized as a named
interface. If it is a weird one off concept and function maybe it is fine to
use the anonymous interface approach.

## Conclusion

If we look back at what we did at a high level it looks as follows.

- question the belonging of some code
- identify that it is **not** owned naturally by the class
- extract it to a parameterized function
- decide to parameterize with an interface (either anonymous/named) or leave it multiple parameters

This process is always the same. You just rinse and repeat the above steps over
and over again as you are coding, and you will end up with a far more robust,
meaningful, and reusable code base.
