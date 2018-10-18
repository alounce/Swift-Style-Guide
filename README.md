# Swift Style Guide
Coding Style Guide for Swift Language

Before reviewing some specific cases please read [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) for the Swift language.

## Table Of Contents

- [Swift Style Guide](#swift-style-guide)
    - [0. SwiftLint usage](#0-swiftlint-usage)
	- [1. Code Formatting](#1-code-formatting)
		- [1.1 Spacing](#11-spacing)
		- [1.2 Length of lines](#12-length-of-lines)
		- [1.3 Braces](#13-braces)
		- [1.4 Indentation](#14-indentation)
		- [1.5 Function declaration](#15-function-declaration)
		- [1.6 Function invocation](#16-function-invocation) 
		- [1.7 Conditions](#17-conditions)  
	- [2. Code Organization](#2-code-organization)
		- [2.1 Extensions](#21-extensions)
		- [2.2 Protocol Conformance](#22-protocol-conformance)
		- [2.3 Unused Code](#23-unused-code) 
		- [2.4 Imports](#24-imports)
	- [3. Naming](#3-naming)
	- [4. Coding Style](#4-coding-style)
		- [4.1 General](#41-General)
		- [4.2 Access Modifiers](#42-access-modifiers)
		- [4.3 Custom Operators](#43-custom-operators)
		- [4.4 Switch Statements and enums](#44-switch-statements-and-enums)
		- [4.5 Optionals](#45-0ptionals)
		- [4.6 Protocols](#46-protocols)
		- [4.7 Constants](#47-constants)
		- [4.8 Properties](#48-properties)
		- [4.9 Closures](#49-closures)
        - [4.10 Ternary operator](#410-ternary-operator)
		- [4.11 Arrays](#411-arrays)
		- [4.12 Using guard](#412-using-guard)
		- [4.13 Golden Path](#413-golden-path)
	- [5. Memory Management](#5-memory-management)
	- [6. Copyright Statement](#6-copyright-statement)
    

## 0. SwiftLint usage
For automating some coding standard validation use [SwiftLint](https://github.com/realm/SwiftLint) which checks following [rules](https://github.com/realm/SwiftLint/blob/master/Rules.md). 
At the beginning, configure linter to use most strict mode as possible and then gradually team can relax it by disabling some rules.
*(following guide does not contain some rules which are controlled by SwiftLint)*


## 1. Code Formatting

### 1.1 Spacing
Keep tab and spaces as Xcode defaults: `Tab width`: **4 spaces** and `Indent width`: **4 spaces**.

### 1.2 Length of lines
Try to avoid long lines with a hard maximum of 160 characters per line (Xcode->Preferences->Text Editing->Page guide at column: 160 is helpful for this)

### 1.3 Braces
For constructions like `if/else/switch/while` always open on the same line as the statement but close on a new line. [1TBS style](https://en.m.wikipedia.org/wiki/Indentation_style#1TBS)

```swift
// PREFERRED
class MyClass {
    func myMethod() {
        if flag {
            // Do something
        } else if otherFlag {
            // Do something else
        } else {
            // Do something really else
        }
    }
}

// NOT PREFERRED
class MyClass 
{
    func myMethod() 
    {
        if flag 
        {
            // Do something
        } 
        else if otherFlag 
        {
            // Do something else
        } 
        else 
        {
            // Do something really else
        }
    }
}
```

### 1.4 Indentation 

Use Xcode's recommended indentation style. 
Code should not change if _Editor->Structure->Re-Indent_ (^I) is pressed. 

```swift
// Xcode indentation for a function declaration that spans multiple lines
func multiArgumentFunction(argumentOne: String,
                           argumentTwo: String,
                           argumentThree: String) -> String {
    // Xcode indents to here for this kind of statement
    print("Doing somehting useful here!")
}

// Xcode indentation for a multi-line `if` statement
if firstLongLongCondition
    && secondLongLongCondition
    && thirdLongLongCondition {

    // Xcode indents to here for this kind of statement
    print("Doing somehting useful here!")
}
```

### 1.5 Function declaration 

Keep short function declarations on one line including the opening brace, however for functions with long signatures, put arguments extra indent on subsequent lines:

```swift
// Short function declaration
func reticulateSplines(spline: [Double]) -> Bool {
  // reticulate code goes here
}

// Long function declaration
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool {
  // reticulate code goes here
}

// Another apporach of long function declaration, even more preferrable 
func reticulateSplines(spline: [Double], 
                       adjustmentFactor: Double,
                       translateConstant: Int, 
                       comment: String) -> Bool {
  // reticulate code goes here
}
```


### 1.6 Function invocation 

When calling a function that has many parameters, put each argument on a separate line with a single extra indentation.

```swift
let result = multiArgumentFunction(
    argumentOne: "James",
    argumentTwo: "Bond",
    argumentThree: getAnAddressString())
```

When dealing with an implicit array or dictionary large enough to warrant splitting it into multiple lines, treat the `[` and `]` as if they were braces in a method, `if` statement, etc. Closures in a method should be treated similarly.

```swift
functionWithSomeArguments(
    stringArgument: "hello I am a string",
    arrayArgument: [
        "first",
        "second"
    ],
    dictionaryArgument: [
        "key1": "value1",
        "key2": "value2"
    ],
    someClosure: { argument in
        print(argument)
    })
```

### 1.7 Conditions  

Prefer using local constants or other mitigation techniques to avoid multi-line predicates where possible.

```swift
// PREFERRED
let firstCondition = (A == F || A > M) && A < Const.Threshold 
let secondCondition = (B == D || B > K) && B > Const.Threshold 
let thirdCondition = C != E || C < M 
if firstCondition && secondCondition && thirdCondition {
    // do something
}

// NOT PREFERRED
if ((A == F || A > M) && A < Const.Threshold) 
    && ((B == D || B > K) && B > Const.Threshold) 
    && (C != E || C < M )  {
    // do something
}
```



## 2. Code Organization

### 2.1 Extensions 

Use extensions to organize your code into logical blocks of functionality. Each extension should be set off with a 

`// MARK: - comment to keep things well-organized.`

### 2.2 Protocol Conformance 

In particular, when adding protocol conformance to a model, prefer adding a separate extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.
```swift
// PREFERRED
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}

// NOT PREFERRED
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

### 2.3 Unused Code 

Unused (dead) code, including Xcode template code and placeholder comments should be removed. An exception is when this code can be used in future - however then provide proper reason explaination in the comments.

### 2.4 Imports 

Keep imports minimal. For example, don't import UIKit when importing Foundation will suffice.

## 3. Naming

**3.1** - Do not use class prefixing in Swift
```swift
// PREFERRED
class Account { ... }
// NOT PREFERRED
class SGAccount { ... }
```

**3.2** - Use `PascalCase` for type names

**3.3** - Use `camelCase` for function, method, property, constant, variable, argument names, enum cases, etc.

**3.4** - When dealing with an acronym or other name that is usually written in all caps, actually use all caps in any names that use this in code. The exception is if this word is at the start of a name that needs to start with lowercase - in this case, use all lowercase for the acronym.

```swift
// "HTML" is at the start of a constant name, so we use lowercase "html"
let htmlBodyContent: String = "<p>Hello, World!</p>"
// Prefer using ID to Id
let profileID: Int = 1
// Prefer URLFinder to UrlFinder
class URLFinder {
    /* ... */
}
```

**3.5** - All constants that are instance-independent should be `static`. All such `static` constants should be placed in a marked section of their `class`, `struct`, or `enum`. For classes with many constants, you should group constants that have similar or the same prefixes, suffixes and/or use cases.

```swift
// PREFERRED    
class ClientViewController {
    // MARK: - Constants
    static let buttonPadding: CGFloat = 20.0
    static let shared = ClientViewController()
}

// NOT PREFERRED
class ClientViewController {
    // Don't use `k`-prefix
    static let kButtonPadding: CGFloat = 20.0
}
```

**3.6** - Names should be descriptive and unambiguous.

```swift
// PREFERRED
class RoundAnimatingButton: UIButton { /* ... */ }

// NOT PREFERRED
class CustomButton: UIButton { /* ... */ }
```

**3.7** - Do not abbreviate, use shortened names, or single letter names.

```swift
// PREFERRED
class RoundAnimatingButton: UIButton {
    let animationDuration: NSTimeInterval

    func startAnimating() {
        let firstSubview = subviews.first
    }

}

// NOT PREFERRED
class RoundAnimating: UIButton {
    let aniDur: NSTimeInterval

    func srtAnmating() {
        let v = subviews.first
    }
}
```

**3.8** - Include type information in constant or variable names when it is not obvious otherwise.

But try to keep things simple!

```swift
// PREFERRED

// though not preferred, for some cases its ok to have ConnectionTableViewCell, 
// however it could be enough just ConnectionCell
class ConnectionTableViewCell: UITableViewCell {
    let personImageView: UIImageView

    let animationDuration: TimeInterval

    // It is OK and preferable to use `Controller` instead of `ViewController`
    let popupController: UIViewController

    // when working with outlets, make sure to specify the outlet type in the
    // property name.
    @IBOutlet weak var okButton: UIButton!
    @IBOutlet weak var emailTextField: UITextField!
    @IBOutlet weak var nameLabel: UILabel!

}

// NOT PREFERRED
class ConnectionTableViewCell: UITableViewCell {
    // this isn't a `UIImage`, so shouldn't be called image
    // use personImageView instead
    let personImage: UIImageView

    // this isn't a `String`, so it should be `textLabel`
    let text: UILabel

    // `animation` is not clearly a time interval
    // use `animationDuration` or `animationTimeInterval` instead
    let animation: TimeInterval

    // this is not obviously a `String`
    // use `transitionText` or `transitionString` instead
    let transition: String

    // this is a view controller - not a view
    let popupView: UIViewController

    // don't use abbreviations like
    // `VC` instead of `ViewController`
    let popupVC: UIViewController

    // for the sake of consistency, we should put the type name at the end of the
    // property name and not at the start
    @IBOutlet weak var btnSubmit: UIButton!
    @IBOutlet weak var buttonSubmit: UIButton!

    // we should always have a type in the property name when dealing with outlets
    // for example, here, we should have `firstNameLabel` instead
    @IBOutlet weak var firstName: UILabel!
}
```

**3.9** - When naming function arguments, make sure that the function can be read easily to understand the purpose of each argument.

**3.10** - As per [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/), a `protocol` should be named as nouns if they describe what something is doing (e.g. `Collection`) and using the suffixes `able`, `ible`, or `ing` if it describes a capability (e.g. `Equatable`, `ProgressReporting`). If neither of those options makes sense for your use case, you can add a `Protocol` suffix to the protocol's name as well. Some example `protocol`s are below.

```swift
// here, the name is a noun that describes what the protocol does
protocol TableViewSectionProvider {
    func rowHeight(at row: Int) -> CGFloat
    var numberOfRows: Int { get }
    /* ... */
}

// here, the protocol is a capability, and we name it appropriately
protocol Loggable {
    func logCurrentState()
    /* ... */
}

// suppose we have an `InputTextView` class, but we also want a protocol
// to generalize some of the functionality - it might be appropriate to
// use the `Protocol` suffix here
protocol InputTextViewProtocol {
    func sendTrackingEvent()
    func inputText() -> String
    /* ... */
}
```

## 4. Coding Style

### 4.1 General

**4.1.1** -  Prefer the composition of `map`, `filter`, `reduce`, etc. over iterating when transforming from one collection to another. Make sure to avoid using closures that have side effects when using these methods.

```swift
// PREFERRED
let stringOfInts = [1, 2, 3].flatMap { String($0) }
// ["1", "2", "3"]

// NOT PREFERRED
var stringOfInts: [String] = []
for integer in [1, 2, 3] {
    stringOfInts.append(String(integer))
}

// PREFERRED
let evenNumbers = [4, 8, 15, 16, 23, 42].filter { $0 % 2 == 0 }
// [4, 8, 16, 42]

// NOT PREFERRED
var evenNumbers: [Int] = []
for integer in [4, 8, 15, 16, 23, 42] {
    if integer % 2 == 0 {
        evenNumbers.append(integer)
    }
}
```

**4.1.2** -  Prefer not declaring types for constants or variables if they can be inferred anyway.
```swift
// PREFERRED
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)

// NOT PREFERRED
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```
**4.1.3** - If a function returns multiple values, prefer returning a tuple to using `inout` arguments (it’s best to use labeled tuples for clarity on what you’re returning if it is not otherwise obvious). If you use a certain tuple more than once, consider using a `typealias`. If you’re returning 3 or more items in a tuple, consider using a `struct` or `class` instead.

```swift
func encode(_ clientJSON: [Any: String]]) -> (client: Client, error: Error?) {
    ...
    return (result, error)
}

let operation = encode(data)
guard let error = operation.error {
    ...
}
let client = operation.client

```

**4.1.4** - Avoid writing out an `enum` type where possible - use shorthand.

```swift
// PREFERRED
imageView.setImageWithURL(url, type: .person)

// NOT PREFERRED
imageView.setImageWithURL(url, type: AsyncImageView.Type.person)
```

**4.1.5** - Don’t use shorthand for class methods since it is generally more difficult to infer the context from class methods as opposed to `enum`s.

```swift
// PREFERRED
imageView.backgroundColor = UIColor.white

// NOT PREFERRED
imageView.backgroundColor = .white
```

**4.1.6** - Prefer not writing `self.` unless it is required (for example in closures).

**4.1.7** - When using a statement such as `else`, `catch`, etc. that follows a block, put this keyword on the same line as the block. Again, we are following the [1TBS style](https://en.m.wikipedia.org/wiki/Indentation_style#1TBS) here. Example `if`/`else` and `do`/`catch` code is below.

```swift
if someBoolean {
    // do something
} else {
    // do something else
}

do {
    let fileContents = try readFile("filename.txt")
} catch {
    print(error)
}
```

**4.1.8** - Prefer `static` to `class` when declaring a function or property that is associated with a class as opposed to an instance of that class. Only use `class` if you specifically need the functionality of overriding that function or property in a subclass, though consider using a `protocol` to achieve this instead.

**4.1.9** - If you have a function that takes no arguments, has no side effects, and returns some object or value, prefer using a computed property instead.

### 4.2 Access Modifiers

**4.2.1** - Write the access modifier keyword first if it is needed.

```swift
// PREFERRED
private static let myPrivateNumber: Int

// NOT PREFERRED
static private let myPrivateNumber: Int
```

**4.2.2** - If a property needs to be accessed by unit tests, you will have to make it `internal` to use `@testable import ModuleName`. If a property *should* be private, but you declare it to be `internal` for the purposes of unit testing, make sure you add an appropriate bit of documentation commenting that explains this. You can make use of the `- warning:` markup syntax for clarity as shown below.

```swift
/**
 This property defines the pirate's name.
 - warning: Not `private` for `@testable`.
 */
let pirateName = "LeChuck"
```

**4.2.3** - Prefer `private` to `fileprivate` where possible.

**4.2.4** - When choosing between `public` and `open`, prefer `open` if you intend for something to be subclassable outside of a given module and `public` otherwise. Note that anything `internal` and above can be subclassed in tests by using `@testable import`, so this shouldn't be a reason to use `open`. In general, lean towards being a bit more liberal with using `open` when it comes to libraries, but a bit more conservative when it comes to modules in a codebase such as an app where it is easy to change things in multiple modules simultaneously.

### 4.3 Custom Operators

Prefer creating named functions to custom operators.

If you want to introduce a custom operator, make sure that you have a *very* good reason why you want to introduce a new operator into global scope as opposed to using some other construct.

You can override existing operators to support new types (especially `==`). However, your new definitions must preserve the semantics of the operator. For example, `==` must always test equality and return a boolean.

### 4.4 Switch Statements and enums

**4.4.1** - When using a switch statement that has a finite set of possibilities (`enum`), do *NOT* include a `default` case. Instead, place unused cases at the bottom and use the `break` keyword to prevent execution.

**4.4.2** - Since `switch` cases in Swift break by default, do not include the `break` keyword if it is not needed.

**4.4.3** - When defining a case that has an associated value, make sure that this value is appropriately labeled as opposed to just types (e.g. `case hunger(hungerLevel: Int)` instead of `case hunger(Int)`).

```swift
enum Problem {
    case attitude
    case hair
    case hunger(hungerLevel: Int)
}

func handleProblem(problem: Problem) {
    switch problem {
    case .attitude:
        print("At least I don't have a hair problem.")
    case .hair:
        print("Your barber didn't know when to stop.")
    case .hunger(let hungerLevel):
        print("The hunger level is \(hungerLevel).")
    }
}
```

**4.4.4** - Prefer lists of possibilities (e.g. `case 1, 2, 3:`) to using the `fallthrough` keyword where possible).

**4.4.5** - If you have a default case that shouldn't be reached, preferably throw an error (or handle it some other similar way such as asserting).

```swift
func handleDigit(_ digit: Int) throws {
    switch digit {
    case 0, 1, 2, 3, 4, 5, 6, 7, 8, 9:
        print("Yes, \(digit) is a digit!")
    default:
        throw Error(message: "The given number was not a digit.")
    }
}
```

### 4.5 Optionals

**4.5.1** - Try to avoid using `as!` or `try!`. Use them only if you have a reason, and with proper comment above this line

**4.5.2** - If you don't plan on actually using the value stored in an optional, but need to determine whether or not this value is `nil`, explicitly check this value against `nil` as opposed to using `if let` syntax.

```swift
// PREFERERED
if someOptional != nil {
    // do something
}

// NOT PREFERRED
if let _ = someOptional {
    // do something
}
```

**4.5.3** - Prefer using `weak` to using `unowned`. You can think of `unowned` as somewhat of an equivalent of a `weak` property that is implicitly unwrapped (though `unowned` has slight performance improvements on account of completely ignoring reference counting). Since we don't ever want to have implicit unwraps, we similarly don't want `unowned` properties.


**4.5.4** -  When unwrapping optionals, use the same name *(shadow the original name)* for the unwrapped constant or variable where appropriate.

```swift
guard let error = error else {
    return
}
```
### 4.6 Protocols

When implementing protocols, there are two ways of organizing your code:

1. Using `// MARK:` comments to separate your protocol implementation from the rest of your code
2. Using an extension outside your `class`/`struct` implementation code, but in the same source file

Keep in mind that when using an extension, however, the methods in the extension can't be overridden by a subclass, which can make testing difficult. If this is a common use case, it might be better to stick with method #1 for consistency. Otherwise, method #2 allows for cleaner separation of concerns.

Even when using method #2, add `// MARK:` statements anyway for easier readability in Xcode's method/property/class/etc. list UI.

### 4.7 Constants

You can define constants on a type rather than on an instance of that type using type properties. To declare a type property as a constant simply use static let. Type properties declared in this way are generally preferred over global constants because they are easier to distinguish from instance properties. Example:

```swift
// PREFERRED
enum Math {
  static let e = 2.718281828459045235360287
  static let root2 = 1.41421356237309504880168872
}

let hypotenuse = side * Math.root2

// NOT PREFERRED
let e = 2.718281828459045235360287  // pollutes global namespace
let root2 = 1.41421356237309504880168872

let hypotenuse = side * root2 // what is root2?
```


### 4.8 Properties

**4.8.1** - You can declare a singleton property as follows:

```swift
class PirateManager {
    static let shared = PirateManager()

    /* ... */
}
```

### 4.9 Closures

**4.9.1** - Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.

```swift
// PREFERRED
UIView.animate(withDuration: 1.0) {
  self.myView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}, completion: { finished in
  self.myView.removeFromSuperview()
})

// NOT PREFERRED
UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}) { f in
  self.myView.removeFromSuperview()
}
```

**4.9.2** - If the types of the parameters are obvious, it is OK to omit the type name, but being explicit is also OK. Sometimes readability is enhanced by adding clarifying detail and sometimes by taking repetitive parts away - use your best judgment and be consistent.

```swift
// omitting the type
doSomethingWithClosure() { response in
    print(response)
}

// explicit type
doSomethingWithClosure() { response: NSURLResponse in
    print(response)
}

// using shorthand in a map statement
[1, 2, 3].flatMap { String($0) }
```

**4.9.3** - If specifying a closure as a type, you don’t need to wrap it in parentheses unless it is required (e.g. if the type is optional or the closure is within another closure). Always wrap the arguments in the closure in a set of parentheses - use `()` to indicate no arguments and use `Void` to indicate that nothing is returned.

```swift
let completionBlock: (Bool) -> Void = { (success) in
    print("Success? \(success)")
}

let completionBlock: () -> Void = {
    print("Completed!")
}

let completionBlock: (() -> Void)? = nil
```

**4.9.4** - Keep parameter names on same line as the opening brace for closures when possible without too much horizontal overflow (i.e. ensure lines are less than 160 characters).

**4.9.5** - Use trailing closure syntax unless the meaning of the closure is not obvious without the parameter name (an example of this could be if a method has parameters for success and failure closures).

```swift
// trailing closure
doSomething(1.0) { (parameter1) in
    print("Parameter 1 is \(parameter1)")
}

// no trailing closure
doSomething(1.0, success: { (parameter1) in
    print("Success with \(parameter1)")
}, failure: { (parameter1) in
    print("Failure with \(parameter1)")
})
```

### 4.10 Ternary operator
The ternary operator `condition ? if-yes : if-no` should only be used when it increases clarity of the code. Awoid using nested ternary operations.

```swift
// PREFERRED

let value = 5
result = value > 0 ? x : y

let isHorizontal = true
result = isHorizontal ? x : y

// NOT PREFERRED
result = a > b ? c > d ? x > y ? b : d : y : x
```

### 4.11 Arrays

**4.11.1** - In general, avoid accessing an array directly with subscripts. When possible, use accessors such as `.first` or `.last`, which are optional and won’t crash. Prefer using a `for item in items` syntax when possible as opposed to something like `for i in 0 ..< items.count`. If you need to access an array subscript directly, make sure to do proper bounds checking. You can use `for (index, value) in items.enumerated()` to get both the index and the value.

**4.11.2** - Never use the `+=` or `+` operator to append/concatenate to arrays. Instead, use `.append()` or `.append(contentsOf:)` as these are far more performant (at least with respect to compilation) in Swift's current state. If you are declaring an array that is based on other arrays and want to keep it immutable, instead of `let myNewArray = arr1 + arr2`, use `let myNewArray = [arr1, arr2].joined()`.



### 4.12 Using guard

**4.12.1** - When unwrapping optionals, prefer `guard` statements as opposed to `if` statements to decrease the amount of nested indentation in your code.

```swift
// PREFERRED
guard let monkeyIsland = monkeyIsland else {
    return
}
bookVacation(on: monkeyIsland)
bragAboutVacation(at: monkeyIsland)

// NOT PREFERRED
if let monkeyIsland = monkeyIsland {
    bookVacation(on: monkeyIsland)
    bragAboutVacation(at: monkeyIsland)
}

// EVEN LESS PREFERRED
if monkeyIsland == nil {
    return
}
bookVacation(on: monkeyIsland!)
bragAboutVacation(at: monkeyIsland!)
```

**4.12.2** - When deciding between using an `if` statement or a `guard` statement when unwrapping optionals is *not* involved, the most important thing to keep in mind is the readability of the code. There are many possible cases here, such as depending on two different booleans, a complicated logical statement involving multiple comparisons, etc., so in general, use your best judgement to write code that is readable and consistent. If you are unsure whether `guard` or `if` is more readable or they seem equally readable, prefer using `guard`.

```swift
// an `if` statement is readable here
if operationFailed {
    return
}

// a `guard` statement is readable here
guard isSuccessful else {
    return
}

// double negative logic like this can get hard to read - i.e. don't do this
guard !operationFailed else {
    return
}
```

**4.12.3** - If choosing between two different states, it makes more sense to use an `if` statement as opposed to a `guard` statement.

```swift
// PREFERRED
if isFriendly {
    print("Hello, nice to meet you!")
} else {
    print("You have the manners of a beggar.")
}

// NOT PREFERRED
guard isFriendly else {
    print("You have the manners of a beggar.")
    return
}

print("Hello, nice to meet you!")
```

**4.12.4** - Often, we can run into a situation in which we need to unwrap multiple optionals using `guard` statements. In general, combine unwraps into a single `guard` statement if handling the failure of each unwrap is identical (e.g. just a `return`, `break`, `continue`, `throw`, or some other `@noescape`).

```swift
// combined because we just return
guard let thingOne = thingOne,
    let thingTwo = thingTwo,
    let thingThree = thingThree else {
    return
}

// separate statements because we handle a specific error in each case
guard let thingOne = thingOne else {
    throw Error(message: "Unwrapping thingOne failed.")
}

guard let thingTwo = thingTwo else {
    throw Error(message: "Unwrapping thingTwo failed.")
}

guard let thingThree = thingThree else {
    throw Error(message: "Unwrapping thingThree failed.")
}
```

**4.12.5** - Don’t use one-liners for `guard` statements.


```swift
// PREFERRED
guard let thingOne = thingOne else {
    return
}

// NOT PREFERRED
guard let thingOne = thingOne else { return }
```

### 4.13 Golden Path

When coding with conditionals, the left-hand margin of the code should be the "golden" or "happy" path. That is, don't nest if statements. Multiple return statements are OK. The guard statement is built for this.

```swift
// PREFERRED
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  guard let context = context else {
    throw FFTError.noContext
  }
  guard let inputData = inputData else {
    throw FFTError.noInputData
  }

  // use context and input to compute the frequencies
  return frequencies
}

// NOT PREFERRED
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  if let context = context {
    if let inputData = inputData {
      // use context and input to compute the frequencies

      return frequencies
    } else {
      throw FFTError.noInputData
    }
  } else {
    throw FFTError.noContext
  }
}
```
When multiple optionals are unwrapped either with guard or if let, minimize nesting by using the compound version when possible. Example:

```swift
// PREFERRED
guard let number1 = number1,
      let number2 = number2,
      let number3 = number3 else {
  fatalError("impossible")
}
// do something with numbers

// NOT PREFERRED
if let number1 = number1 {
  if let number2 = number2 {
    if let number3 = number3 {
      // do something with numbers
    } else {
      fatalError("impossible")
    }
  } else {
    fatalError("impossible")
  }
} else {
  fatalError("impossible")
}
```

## 5. Memory Management

**5.1** - Code (even non-production, tutorial demo code) should not create reference cycles. Analyze your object graph and prevent strong cycles with weak and unowned references. Alternatively, use value types (struct, enum) to prevent cycles altogether.


**5.2** - Extend object lifetime using the `[weak self]` and `guard let ``self`` = self else { return }` idiom. [weak self] is preferred to [unowned self] where it is not immediately obvious that self outlives the closure. Explicitly extending lifetime is preferred to optional unwrapping. In Swift 4.2, you should drop the backticks in the guard statement.

```swift
// PREFERRED
resource.request().onComplete { [weak self] response in
  guard let `self` = self else {
    return
  }
  let model = self.updateModel(response)
  self.updateUI(model)
}

// NOT PREFERRED
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
  let model = self.updateModel(response)
  self.updateUI(model)
}

// NOT PREFERRED
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
  let model = self?.updateModel(response)
  self?.updateUI(model)
}
```

## 6. Copyright Statement
The following copyright statement should be included at the top of every source file:

```swift
//
//  {{ProjectName}}
//
//  Copyright © {{YearOfCreation}} {{Company}}. All rights reserved.
//
```