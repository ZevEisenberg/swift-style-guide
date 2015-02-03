# Swift Style Guide

This guide is designed to help maintain consistency in iOS projects. Some preferences are subjective and can be tweaked if needed, however if you decide to adopt them it is best not to deviate.

## Table of contetns

* [self](#self)
* [let vs var](#let-vs-var)
* [Operands](#operands)
* [Optionals](#optionals)
* [Properties](#properties)
* [Methods](#methods)
* [Conditionals](#conditionals)
* [Ternary Operator](#ternary-operator)
* [Collections](#collections)
* [Error handling](#error-handling)
* [Comments](#comments)
* [Organization](#organization)
	* [MARK](#mark)
	* [Empty lines](#empty-lines)
	* [Extensions](#extensions)

### Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.

## self

* When accessing properties or methods on self, leave the reference to self implicit by default.
* Only include the explicit keyword when required by the language — for example, in a closure, or when parameter names conflict:

```swift
extension History {
    init(events: [Event]) {
        self.events = events
    }

    var whenVictorious: () -> () {
        return {
            self.rewrite()
        }
    }
}
```

## let vs var

Use `let foo = …` over `var foo = …` wherever possible. Only use var if you absolutely have to (i.e. you know that the value might change, e.g. when using the weak storage modifier).

It becomes easier to reason about code. Had you used var while still making the assumption that the value never changed, you would have to manually check that.

Accordingly, whenever you see a var identifier being used, assume that it will change and ask yourself why.

## Operands

Separate binary operands with a single space, but unary operands with none:

```swift
int coefficient = x + y * 10;

for var i = 0; i < 10; i++ {
    // do semething
}
```

## Optionals

* Use `if let …` or optional chaining insead of force-unwrapping(`!`).
* Avoid using implicitly unwrapped optionals.

```swift
if let foo = foo {
    // Use unwrapped `foo` value in here
} else {
    // If appropriate, handle the case where the optional is nil
}

// Call the function if `foo` is not nil. If `foo` is nil, ignore we ever tried to make the call
foo?.callSomethingIfFooIsNotNil()
```

## Properties

* When possible, omit the `get` keyword on read-only computed properties and read-only subscripts.

```swift
var myGreatProperty: Int {
    return 4
}

subscript(index: Int) -> T {
    return objects[index]
}
```

## Methods

* Use descriptive method and parameter names.
* Method braces should open on the same line with declaration and close on new lines.
* Empty methods can have both brackets on the same line with declaration.

```swift
func doSomethingWithThin(thing: SomeType, otherThing: SomeType) {
	// do something
}

func unwindSegue(segue : UIStoryboardSegue) {}
```

## Conditionals

* Return and break early.
* There should be a single space character before and after the condition.
* Braces should open on the same line as the statement and close on a new line.

```swift
if something == 10 {
	// do something
} else {
	// do something else
}
```

Longer or more complex conditions should be split over multiple lines:

```swift
if something
	&& someOtherThing
	&& someThirdThing {
	// do something
}
```

## Ternary Operator

The Ternary operator, `?` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into instance variables.

Ternary operators should be wrapped in parentheses and only used for assignment and arguments.

```swift
result = (a > b ? x : y)
```

## Collections

* Use shorthand notation when creating `Array` and `Dictionary`.
* Long or complex literals should be split over multiple lines.

```swift
let array = [ItemType]()
let dictionary = [KeyType: ValueType]()
```

* Array literals should have one space character between elements, with comma attached to the left item.

```swift
let array = [1, 2, 3]

let array = [
    "some long string",
    "some other long string"
]
```

* Dictionary literals have one space character between key and value with colon attached to the key. There should also be one space character separating key value pairs, with comma attached to the left pair.

```swift
let dictionary = ["key": "value", "key2": "value2"]

let dictionary = [
    "somekey": "corresponds to this value value",
    "someOtherkey": "corresponds to other value"
]
```

## Error handling

* Use exceptions **only** to indicate programmer error.
* To indicate errors, use `NSError`.

When methods return an error parameter by reference, switch on the returned value, not the error variable.

```swift
var error: NSError?

if !trySomethingWithError(&error) {
    if error != nil {
    	// handle error
    }
}
```

## Comments

Code should be as self-documenting as possible, with only the need for intermittent, few-line explanations.

Comments should be kept up-to-date, or deleted.  
When they are needed, comments should be used to explain **why** a particular piece of code does something. 

When necessary, public API should have documentation comments:

```swift
/// Concise documentation comment
func doSomethingWith(object: SomeType) -> Bool {}

/**
	This is a method description
	:param: objectName parameter description
	:returns: return value description
 */
func doSomethingWith(object: SomeType) -> Bool {}
```

## Organization

### MARK:

 * `// MARK: -` to categorize code into functional groupings.
 * `// MARK:` for subgroups.

### Empty lines

* `MARK` sections should be separated from each other by 2 empty lines.
* Use empty lines sparingly to separate code where it improves readability.

### Extensions

* Use extensions when adopting protocols:

```swift
extension SomeClass: SomeProtocol {
	// conform to protocol
}
```