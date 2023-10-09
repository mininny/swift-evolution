# Allow operator functions to have extra arguments with default values

* Proposal: [SE-NNNN](NNNN-filename.md)
* Authors: [Minhyuk Kim](https://github.com/mininny)
* Review Manager: TBD
* Status: **Awaiting implementation** or **Awaiting review**
* Bug: [apple/swift#54301](https://github.com/apple/swift/issues/54301)
* Implementation: [apple/swift#69051](https://github.com/apple/swift/pull/69051)
* Previous Proposal: *if applicable* [SE-XXXX](XXXX-filename.md)
* Review: ([pitch](https://forums.swift.org/...))

## Introduction

Allow operator functions to accept more than their current limitation of one or two arguments, provided that the additional arguments are assigned default values.

## Motivation

Currently, operator functions in Swift can only have exactly one or two arguments depending on the type of the operator. This limitation can be relaxed to offer developers more flexibility, especially when additional parameters can be inferred or provided during function invocation.  

For example, this can be used to add `#file` and `#line` defaults in the operator function for logging purposes. 

## Proposed solution

We would lift the hard-coded limitation of argument count in operator functions.

When the proposal is accepted, an operator function would be allowed to have:
- One or two main parameters without default values.
- Any number of additional parameters, given that they all have default values.

Here's an illustrative example:

 ```swift
 infix operator <-

// Valid under the proposed change due to the presence of a default value for the third parameter
public func <- (lhs: T, rhs :T, file: StaticString = #file) -> U { /**/ }
```

This change doesn't affect the behavior of operator functions as the arguments with default values will be automatically filled in.
 
## Detailed design

The compiler counts the number of arguments of operator functions, and requires them to be:
- A single argument for prefix and postfix operators
- Two arguments for infix operators

The argument counting logic should be modified to exclude the arguments that have default values.

If a user tries to add non-default arguments to operator functions, the compiler will emit the existing error: `operators must have one or two arguments`. 

## Source compatibility

The proposed change is additive and does not affect source compatibility. 

## ABI compatibility

The proposed change is additive and does not affect ABI compatibility. 

## Implications on adoption

This feature can be freely adopted and un-adopted in source
code with no deployment constraints and without affecting source or ABI
compatibility.

## Acknowledgments
