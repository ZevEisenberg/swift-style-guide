# Swift Style Guide

This guide is designed to help maintain consistency in iOS projects. Some preferences are subjective and can be tweaked if needed, however if you decide to adopt them it is best not to deviate.

## Table of contetns

* [self](#self)
* [let vs var](#let-vs-var)
* [Operands](#operands)
* [Optionals](#optionals)
* [Properties](#properties)
* [Methods](#methods)
* [Callbacks](#callbacks)
* [Conditionals](#conditionals)
* [Ternary Operator](#ternary-operator)
* [Collections](#collections)
* [Error handling](#error-handling)
* [Organization](#organization)
	* [Comments](#comments)
	* [Mark](#mark)
	* [Empty lines](#empty-lines)
	* [Extensions](#extensions)

### Indent using 4 spaces. Never indent with tabs. Lines should be no longer than 130 symbols.
#### Be sure to set these preference in Xcode.

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

Separate binary operands with a single space, but unary operands with none.  
With exception for range operators `...`, `..<`.

```swift
var number = x + y * 10;

for _ in 0..<3 {
	number++
}
```

## Optionals

* Use shorthand notation when declaring optionals `?` and implicitly unwrapped optionals `!`. 
* Use `if let …` or optional chaining insead of force-unwrapping(`!`).
* Avoid using implicitly unwrapped optionals whenever possible.

```swift
if let foo = foo {
    // do something with unwrapped `foo`
} else {
    // if appropriate, handle the case where `foo` is nil
}

// Call the function if `foo` is not nil. 
// If `foo` is nil, ignore we ever tried to make the call.
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

* Use closures when initializing properties that require non-trivial setup.
* Use `lazy` keyword where appropriate to defer computations until the value of the property is called for.

```swift
class SomeClass {

	lazy var classProperty: OtherClass = {
		let other = OtherClass()
		other.setting = value
		return other
	}()
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

func unwindSegue(segue: UIStoryboardSegue) {}
```

## Callbacks

* Use `Void` to denote return type for callbacks that return nothing.
* Optional completion callbacks should have default parameter `nil`.

```swift
func domeSomeTaskWithCallback(callback: (() -> Void)? = nil) {
	…
	if let callback = callback {
		callback()
	}
}
```

## Conditionals

* Return and break early.
* There should be a single space character before and after the condition.
* Braces should open on the same line as the statement and close on a new line.
* `else` clause should begin on the same line with `if`'s closing brace.

```swift
func someFunction(valid: Bool) {
	if !valid {
		return
	}
	// do something
}

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

## Organization

### Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something.

* Code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. 
* When necessary, public API should have documentation comments.
* Comments should be kept up-to-date, or deleted. 

### Mark

 * `// MARK: -` to categorize code into functional groupings.
 * `// MARK:` for subgroups.

### Empty lines

#### No:

* Don't use empty lines as padding for bodies of methods and nested types.
* There should be no empty lines before ending braces of types and functions.

#### Yes:

* Use single empty lines sparingly to separate code into logical chunks.
* `// MARK: -` sections should have 2 empty lines above their declaration.
* When declaring `classes`, `structs`, `enums`, `protocols` in global scope, separate them from other code by 2 empty lines.

### Extensions

* Use extensions when adopting protocols.

### Example:

```swift
protocol SomeProtocol {
	func someMethod()
}


class SomeClass {

	/// Concise documentation comment
	func doStuff() {
	}
	
	/**
		This is a method description
		:param: thing parameter description
		:returns: return value description
	 */
	func doComplexStuff(thing: Science) -> [Magnets] {
	}


	// MARK: - Lifecycle

	init() {
	}
	
	convenience init(stuff: StuffType) {
	}
	
	
	// MARK: - Networking
	
	func download() {
	}
}


extension SomeClass: SomeProtocol {
	// conform to protocol
}
```