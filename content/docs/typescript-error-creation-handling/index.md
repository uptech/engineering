+++
title = "TypeScript Error Creation & Handling"
description = "Quick Review with Examples of TypeScript Error Creation & Handling"
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 420
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

```TypeScript
import { Ok, Err, Result } from 'ts-results';

// Here we define a CustomError that all custom errors should extend from so
// that the custom errors don't have to worry about setting their name
// appropriately.
class CustomError extends Error {
	constructor(message: string) {
		super(message)
		this.name = this.constructor.name;
// This is something that we have seen on the web for issues where Babel won't
// set the stack trace properly when the class doesn't extend properly. But
// from our testing this hasn't seemed to be needed and it is also our
// understanding that super() now does this.
//
//         if (typeof Error.captureStackTrace === 'function') {
//             Error.captureStackTrace(this, this.constructor);
//         } else {
//             this.stack = (new Error(message)).stack;
//         }
	}
}

// This is an error type that is useful to have around because if you have a
// function that returns Result<someType, yourErrorType> you don't want that
// function to throw an exception because you are using Result. So inside
// the function you would do something like the following:
//
// try {
// 		...
// } catch (e: any) {
// 		return new Err(new UnhandledError('some message', e))
// }
//
// This allows you to identify it is an unhandled error but still get to the
// internal error object if needed later on.
class UnhandledError extends CustomError {
	origError: Error
	constructor(message: string, origError: Error) {
		super(message)
		this.origError = origError;
	}
}

// The following are examples of defined custom error types as a hierarchy
// for categorization. In this example we have a DashboardEventsError as a
// base category for the errors and then we have MyErrorClassA and
// MyErrorClassB as specific errors within that category.
class DashboardEventsError extends CustomError {}

class MyErrorClassA extends DashboardEventsError {}
class MyErrorClassB extends DashboardEventsError {}

// The following is code I used to vet the functionality of the custom errors
// and make sure that they meet all the expectations of the interface for the
// Error type

const e = new MyErrorClassB('some message');

// - can we get the name and is it the correct custom name
console.log("name: ", e.name);

// - can we get the message
console.log("message: ", e.message);

// - can get stack trace
console.log("message: ", e.stack);

// - can we conditionally check the custom type to programmatically handle specific errors
if (e instanceof MyErrorClassA) {
	console.log("it is an instance of MyErrorClassA");
} else {
	console.log("it is NOT an instance of MyErrorClassA");
}

if (e instanceof MyErrorClassB) {
	console.log("it is an instance of MyErrorClassB");
} else {
	console.log("it is NOT an instance of MyErrorClassB");
}

// - can conditionally check if it is an instance of custom base class type
if (e instanceof DashboardEventsError) {
	console.log("it is an DashboardEventsError");
} else {
	console.log("it is NOT an DashboardEventsError");
}

// - can conditionally check if it is an instance of Error type
if (e instanceof Error) {
	console.log("it is an Error");
} else {
	console.log("it is NOT an Error");
}

// - do we get the custom type name and stack trace when it is thrown
throw e;


// Sadly because of limitations of TypeScript I haven't found a good way yet
// to meet the following criteria. 
// - communicating to the user via the type system what the possible error types are
// - requiring handling of all possible returned errors or explicitly declare not handling
//
// The following are just explorations I started but didn't really go anywhere to meet those requirements


// - communicating to the user via the type system what the possible error types are
//
// typescript doesn't seem to complain even through I declared the possible
// return types that I am returning a different error type. So not really
// useful as a tool for the author of the function. Maybe it could be seen as
// documentation for the consumer
function foo(): Result<any, MyErrorClassA | UnhandledError> {
	try {
		console.log("in try");
		return new Err(new MyErrorClassB('error a'));
	} catch (e: any) {
		console.log("in the catch");
		return new Err(new UnhandledError('message', e));
	}
}


// - requiring handling of all possible returned errors or explicitly declare not handling
//
// I don't believe there is anything in TypeScript to support exhaustive
// handling of error types. I have contemplated having a pairing function with
// each function that enforces this. However, there is nothing to help the
// author of that function make sure they aren't forgetting any error types.
// Also it doesn't help the use case when you are mapping errors in the middle
// of a chain.
//
// So I think for the time being unless we were to look into a TypeScript
// compiler extension we are stuck in this less than ideal world.

let result = foo();
if (result.ok) {
	console.log("result ok");
} else {
	console.log("result err");

	if (e instanceof MyErrorClassA) {
		console.log("it is an instance of MyErrorClassA");
	} else {
		console.log("it is NOT an instance of MyErrorClassA");
	}

	if (e instanceof MyErrorClassB) {
		console.log("it is an instance of MyErrorClassB");
	} else {
		console.log("it is NOT an instance of MyErrorClassB");
	}

}
```

## FAQ

#### Is it common for nearly every function in an application to return a Result to handle potential errors? At which point, if any, would that become unnecessary? It seems like this has become the ideal solution for dealing with breaking errors in the Trans-Serv, but I don’t want to find myself in a position of over-using or defaulting to Results if that is not generally the ideal way to design/build applications.

In reference to, "Is it common for nearly every function in an application to return a Result to handle potential error?" The answer is generally yes it is common to use it everywhere and you generally should. The point at which it becomes un-necessary is the point at which an error is handled programmatically. Usually this is at the bounds of the application (e.g. user interface where error is presented to the user, or where an error is internally programmatically handled). You should be using Result whenever it is possible for the operation you are performing to fail. The only caveat to this is if you are in an async chain (aka a promise chain) and. you want a function to throw an exception explicitly for being used in that context of an async chain. However, generally in these cases the async chain is basically mapped back to a result at the end anyways.

#### At what point does piping, mapping, or promise chaining become “too large”? Is the limit determined on the amount of the logic inside each consecutive call? For example, would it be easier to follow a larger number of chains if it’s purely passing in the next function, as opposed to having chains where logic must be implemented beyond calling a single function (even if it is only a few lines)? Is the latter case a “smell” that the logic should be extracted into a new function to cater to the former?

In reference to "At what point does piping, mapping, or promise chaining become “too large”?" This is a subjective thing and hard to get away from it being subjective because the driver of it is readability and being able to easily understand what is going on in the chain. If I have to read through a chain of 20 different steps to understand what the entire chain is trying to accomplish it is too large. I should be able to quickly look at it and understand and somewhere around up to 5 steps or so is a good rule of thumb. It is worth noting though that if you have a really large chain, you should be collapsing multiple steps into a single step with a name that has meaning using pipe(). Having chains where you hand it a lambda is a smell. The ideal is to be in a situation where your chain is completely composed of named functions. This drastically aids with the understanding of the chain and its intent without having to worry about the details of understanding the implementations of those lambdas. If you feel like you have to do logic beyond a single function it is a "smell" that, that logic should be extracted out into a new function.

#### Ok so new question relating to error handling.  For the stream creation helper function, Jon suggested that we return a FileNotFound error to be specific if a file is not found.  The way our Error handling works currently, everything bubbles up to the TransactionServiceError class which then emits the error with an http error code - if I understand correctly.  If an internal function is using the helper function to transform a file path into a stream, we would want to throw a FileNotFound error - but it would not be an http error of 404, it would be an internal error.  When handled by the calling function it might be transformed into a 404, depending on the context (e.g. if it is interacting with the outside and needs to communicate that the file path is missing), or it might remain an internal matter to be passed along as a generic 500 error for the greater service.   So how do we handle this sort of instance?  I created a FileNotFound error so it would contain the message to be handled internally, and it could theoretically be caught and transformed into a TransactionServiceError code 404 by something that handles it...  but I am not sure if that is how we want to manage this?

Im not certain we want everything to be a TransactionServiceError right, since not everything has an http code. Functions can and should have their own errors to inform the caller of the status via Result. TransactionServiceError was designed in relation to Faktory Jobs consuming failures when calling routes, which is separate from this. The potential transformation is outside the scope of this context. This `FileNotFoundError` could be transformed by the caller (if needed).

#### I would ideally like to catch any errors that might occur when creating the stream - in a try catch or something similar.  Since it's a stream it is a bit more complex, since it has an event to listen to for errors .. which I assume the handler would want to handle in this case.  But in general, if I were to need to catch unexpected errors how would I manage an exception that doesn't conform to the Result signature here? Or should I use a more generic Result signature with Error as the Error type and just pass up exceptions as wrapped errors in the result, for e.g. (and the missing file error would still be of type FileNotFoundError)

The function should try to do its best to prevent rouge exceptions. What we have here in this example is good and if there is a rogue exception then the caller should handle it or not. I dont like the idea of a generic Error type in the signature since its unclear of the intent of the function. Rogue exceptions are well that, rouge. We should do our best and the example code above seems to do the best it can. I’d say make a judgement call and we can go from there.

The one addition I would add to that is that you can handle "rouge" / unhandled errors not by generalizing the return type to use Error but instead by formalizing the concept of Unhandled error as part of the valid error type in the return type.

## Envolving Generic Error Classes

Creating unique error classes for each specific function will result in a lot of different error classes. However, we need to make sure that we formalize explicitly all the possible errors that can result from a function. One obvious way to collapse the number of errors is to make Errors more generic. For example you could have a `FileNotFound` error for one function and you can also have another function use the same `FileNotFound` error. How do you know how to make an error generic or not or if you make it too generic.

I have found that defining the error classes with their associated data helps a lot. It gives you a guide to understand if the associated data makes sense in the other generic cases as well. So the recommendation is to start wtih a specific error definition with associated data and then extrapolate from there into a generic type that can be reused.


