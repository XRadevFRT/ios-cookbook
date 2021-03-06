# Ackee Swift Style Guide

Here are the Swift conventions we conform to when we write our code. 

It is based on official Apple’s & Ray Wenderlich’s style guides. 

This guide is aiming for Swift 3. If you are writing in Swift 2 just conform to applicable rules.

## Table of Contents

## Correctness

Consider warnings to be errors. This rule informs many stylistic decisions such as not to use the `++` or `--` operators, C-style for loops, or strings as selectors.

## Naming

Use descriptive names with camel case for classes, methods, variables, etc. Type names (classes, structures, enumerations and protocols) should be capitalized, while method names and variables should start with a lower case letter.

**Preferred:**

```swift
private let maximumWidgetCount = 100

class WidgetContainer {
    var widgetButton: UIButton
    let widgetHeightPercentage = 0.85
}
```

**Not Preferred:**

```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
    var wBut: UIButton
    let wHeightPct = 0.85
}
```

Abbreviations and acronyms should generally be avoided. Following the Apple Design Guidelines, abbreviations and initialisms that appear in all uppercase should be uniformly uppercase or lowercase. Examples:

**Preferred**
```swift
let urlString: URLString
let userID: UserID
```

**Not Preferred**
```swift
let uRLString: UrlString
let userId: UserId
```

For functions and init methods, prefer named parameters for all arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

```swift
func dateFromString(dateString: String) -> Date
func convertPointAt(column column: Int, row: Int) -> CGPoint
func timedAction(afterDelay delay: NSTimeInterval, perform action: SKAction) -> SKAction!

// would be called like this:
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(afterDelay: 1.0, perform: someOtherAction)
```

For methods, follow the standard Apple convention of referring to the first parameter in the method name:

```swift
class Counter {
    func combineWith(otherCounter: Counter, options: Dictionary?) { ... }
    func incrementBy(amount: Int) { ... }
}
```

### Protocols

Following Apple's API Design Guidelines, protocols names that describe what something is should be a noun. Examples: `Collection`, `WidgetFactory`. Protocols names that describe an ability should end in -ing, -able, or -ible. Examples: `Equatable`, `Resizing`.

**Here we have HUGE Ackee exception**

> When creating Protocols for ViewControllers and ViewModels we use -ing ending. It is bit clumsy but we rather have protocol `RegistrationViewModeling` and implementation `RegistrationViewModel` than `RegistrationViewModelImpl` or something like that. 


### Enumerations

Following Apple's API Design Guidelines for Swift 3, use lowerCamelCase for enumeration values.

```swift
enum Shape {
    case rectangle
    case square
    case triangle
    case circle
}
```


### Class Prefixes

Swift types are all automatically namespaced by the module that contains them. As a result, prefixes are not required in order to minimize naming collisions. If two names from different modules collide you can disambiguate by prefixing the type name with the module name:

Old code does not have to be refactored

```swift
import MyModule

var myClass = MyModule.MyClass()
```

You **should not** add prefixes to your Swift types.

If you need to expose a Swift type for use within Objective-C you can provide a suitable prefix as follows:

```swift
@objc (RWTChicken) class Chicken {
   ...
}
```

### Selectors

Selectors are Obj-C methods that act as handlers for many Cocoa and Cocoa Touch APIs. Prior to Swift 2.2, they were specified using type unsafe strings. This now causes a compiler warning. The "Fix it" button replaces these strings with the **fully qualified** type safe selector. Often, however, you can use context to shorten the expression. This is the preferred style.

**Preferred:**
```swift
let sel = #selector(viewDidLoad)
```

**Not Preferred:**
```swift
let sel = #selector(ViewController.viewDidLoad)
```

### Generics

Generic type parameters should be descriptive, upper camel case names. When a type name doesn't have a meaningful relationship or role, use a traditional single uppercase letter such as `T`, `U`, or `V`.

**Preferred:**
```swift
struct Stack<Element> { ... }
func writeTo<Target: OutputStream>(inout target: Target)
func max<T: Comparable>(x: T, _ y: T) -> T
```

**Not Preferred:**
```swift
struct Stack<T> { ... }
func writeTo<target: OutputStream>(inout t: target)
func max<Thing: Comparable>(x: Thing, _ y: Thing) -> Thing
```

## Code Organization

Use extensions to organize your code into logical blocks of functionality. Each extension should be set off with a `// MARK: -` comment to keep things well-organized.

### Protocol Conformance

 In particular, when adding protocol conformance to a model, prefer adding a separate extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

> There is another exception if your conformance implementation requires additional variable, you can conform to that protocol at the end of implementation with `MARK` label.

**Preferred:**
```swift
class MyViewcontroller: UIViewController {
    // class stuff here
}

// MARK: - UITableViewDataSource

extension MyViewcontroller: UITableViewDataSource {
    // table view data source methods
}

// MARK: - UIScrollViewDelegate

extension MyViewcontroller: UIScrollViewDelegate {
    // scroll view delegate methods
}
```

**Not Preferred:**
```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
    // all methods
}
```

Since the compiler does not allow you to re-declare protocol conformance in a derived class, it is not always required to replicate the extension groups of the base class. This is especially true if the derived class is a terminal class and a small number of methods are being overriden. When to preserve the extension groups is left to the discretion of the author.

For UIKit view controllers, consider grouping lifecyle, custom accessors, and `IBAction` in separate class extensions.

### Unused Code

Unused (dead) code, including Xcode template code and placeholder comments should be removed. An exception is when your tutorial or book instructs the user to use the commented code.

Aspirational methods not directly associated with the tutorial whose implementation simply calls the super class should also be removed. This includes any empty/unused UIApplicationDelegate methods.

**Not Preferred:**
```swift
override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
    // Dispose of any resources that can be recreated.
}

override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
    // #warning Incomplete implementation, return the number of sections
    return 1
}

override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    // #warning Incomplete implementation, return the number of rows
    return Database.contacts.count
}

```

**Preferred:**
```swift
override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return Database.contacts.count
}
```

### Minimal Imports

Keep imports minimal. For example, don't import `Foundation` when importing `UIKit` will suffice.

## Spacing and Indentation

* Indent using 4 spaces rather than tabs to conserve space and help prevent line wrapping. This should be configured on the project.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu).

**Preferred:**
```swift
if user.isHappy {
    // Do something
} else {
    // Do something else
}
```

**Not Preferred:**
```swift
if user.isHappy
{
    // Do something
}
else {
    // Do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.
* Colons always have no space on the left and one space on the right. Exceptions are the ternary operator `? :` and empty dictionary `[:]`.

**Preferred:**
```swift
class TestDatabase: Database {
    var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**Not Preferred:**
```swift
class TestDatabase : Database {
    var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

## Classes and Structs

### Which one to use?

Remember, structs have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structs for things that do not have an identity. An array that contains `[a, b, c]` is really the same as another array that contains `[a, b, c]` and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

Sometimes, things should be structs but need to conform to `Any` or are historically modeled as classes already (`NSDate`, `NSSet`). Try to follow these guidelines as closely as possible.

### Example definition

Here's an example of a well-styled class definition:

```swift
class Circle: Shape {
    var x: Int, y: Int
    var radius: Double
    var diameter: Double {
        get {
            return radius * 2
        }
        set {
            radius = newValue / 2
        }
    }

    init(x: Int, y: Int, radius: Double) {
        self.x = x
        self.y = y
        self.radius = radius
    }

    convenience init(x: Int, y: Int, diameter: Double) {
        self.init(x: x, y: y, radius: diameter / 2)
    }
    
    func describe() -> String {
        return "I am a circle at \(centerString()) with an area of \(computeArea())"
    }

    override func computeArea() -> Double {
        return M_PI * radius * radius
    }

    private func centerString() -> String {
        return "(\(x),\(y))"
    }
}
```
The example above demonstrates the following style guidelines:

 - Specify types for properties, variables, constants, argument declarations and other statements with a space after the colon but not before, e.g. `x: Int`, and `Circle: Shape`.
 - Define multiple variables and structures on a single line if they share a common purpose / context.
 - Indent getter and setter definitions and property observers.
 - Don't add modifiers such as `internal` when they're already the default. Similarly, don't repeat the access modifier when overriding a method



### Use of Self

For conciseness, avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Use `self` when required to differentiate between property names and arguments in initializers, and when referencing properties in closure expressions (as required by the compiler):

```swift
class BoardLocation {
    let row: Int, column: Int

    init(row: Int, column: Int) {
        self.row = row
        self.column = column
        
        let closure = {
            print(self.row)
        }
    }
}
```

**Preferred:**

```swift
if let textContainer = textContainer {
    // do many things with textContainer
}
```

**Not Preferred:**

```swift
if let textContainer = self.textContainer {
    // do many things with textContainer
}
```

```swift
if let maybeThisCouldBeTextContainer = textContainer {
    // do many things with maybeThisCouldBeTextContainer
}
```

- To differentiate between property names and arguments in initializers
- When referencing properties in closure expressions

```swift
init(row: Int, column: Int) {
    self.row = row
    self.column = column
    
    let closure = {
        println(self.row)
    }
}
```

**tl;dr**
Only use `self` when the language requires it.

### Computed Properties

For conciseness, if a computed property is read-only, omit the get clause. The get clause is required only when a set clause is provided.

**Preferred:**
```swift
var diameter: Double {
    return radius \* 2.0
}
```

**Not Preferred:**
```swift
var diameter: Double {
    get {
        return radius * 2.0
    }
}
```

## Function Declarations

Keep short function declarations on one line including the opening brace:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

For functions with long signatures, add line breaks at appropriate points and add an extra indent on subsequent lines:

```swift
func reticulateSplines(
    spline: [Double],
    adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool
{
    // reticulate code goes here
}
```


## Closure Expressions

Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.

**Preferred:**
```swift
UIView.animateWithDuration(1.0) {
    self.myView.alpha = 0
}

UIView.animateWithDuration(1.0,
    animations: {
        self.myView.alpha = 0
    },
    completion: { finished in
        self.myView.removeFromSuperview()
    }
)
```

**Not Preferred:**
```swift
UIView.animateWithDuration(1.0, animations: {
    self.myView.alpha = 0
})

UIView.animateWithDuration(1.0,
    animations: {
        self.myView.alpha = 0
    }) { f in
        self.myView.removeFromSuperview()
}
```

For single-expression closures where the context is clear, use implicit returns:

```swift
attendeeList.sort { a, b in
    a > b
}
```

Chained methods using trailing closures should be clear and easy to read in context. Decisions on spacing, line breaks, and when to use named versus anonymous arguments is left to the discretion of the author. Examples:

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.indexOf(90)

let value = numbers
   .map {$0 * 2}
   .filter {$0 > 50}
   .map {$0 + 10}
```

## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Preferred:**
```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Not Preferred:**
```swift
let width: NSNumber = 120.0                                 // NSNumber
let widthString: NSString = width.stringValue               // NSString
```

In Sprite Kit code, use `CGFloat` if it makes the code more succinct by avoiding too many conversions.


### Constants

Constants are defined using the `let` keyword, and variables with the `var` keyword. Always use `let` instead of `var` if the value of the variable will not change.

**Tip:** A good technique is to define everything using `let` and only change it to `var` if the compiler complains!

You can define constants on a type rather than an instance of that type using type properties. To declare a type property as a constant simply use `static let`. Type properties declared in this way are generally preferred over global constants because they are easier to distinguish from instance properties. Example:

**Preferred:**
```swift
enum Math {
    static let e  = 2.718281828459045235360287
    static let pi = 3.141592653589793238462643
}

radius * Math.pi * 2 // circumference
```
**Note:** The advantage of using a case-less enumeration is that it can't accidentally be instantiated and works as a pure namespace.

### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Use implicitly unwrapped types declared with `!` only for instance variables that you know will be initialized later before use, such as subviews that will be set up in `viewDidLoad`.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```swift
textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations:

```swift
if let textContainer = textContainer {
    // do many things with textContainer
}
```

Use `guard` unwrapping if the object is required for continuing the operation.
`guard` is prefered when doing early returns inside of a function.

```swift
guard let requiredObject = object else { return }
```

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, shadow the original name when appropriate rather than using names like `unwrappedView` or `actualLabel`.

**Preferred:**
```swift
var subview: UIView?

// later on...
if let subview = subview {
    // do something with unwrapped subview
}
```

**Not Preferred:**
```swift
var optionalSubview: UIView?

if let unwrappedSubview = optionalSubview {
    // do something with unwrappedSubview
}
```

### Struct Initializers

Use the native Swift struct initializers rather than the legacy CGGeometry constructors.

**Preferred:**
```swift
let bounds = CGRect(x: 40.0, y: 20.0, width: 120.0, height: 80.0)
var centerPoint = CGPoint(x: 96.0, y: 42.0)
```

**Not Preferred:**
```swift
let bounds = CGRectMake(40.0, 20.0, 120.0, 80.0)
var centerPoint = CGPointMake(96.0, 42.0)
```

Prefer the struct-scope constants `CGRect.infiniteRect`, `CGRect.nullRect`, etc. over global constants `CGRectInfinite`, `CGRectNull`, etc. For existing variables, you can use the shorter `.zeroRect`.

### Lazy Initialization

Consider using lazy initialization for finer grain control over object lifetime. This is especially true for `UIViewController` that loads views lazily. You can either use a closure that is immediately called `{ }()` or call a private factory method. Example:

```swift
lazy var locationManager: CLLocationManager = self.makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
    let manager = CLLocationManager()
    manager.desiredAccuracy = kCLLocationAccuracyBest
    manager.delegate = self
    manager.requestAlwaysAuthorization()

    return manager
}
```

**Notes:**
  - `[unowned self]` is not required here. A retain cycle is not created.
  - Location manager has a side-effect for popping up UI to ask the user for permission so fine grain control makes sense here.

### Type Inference

The Swift compiler is able to infer the type of variables and constants. You can provide an explicit type via a type alias (which is indicated by the type after the colon), but in the majority of cases this is not necessary.

Prefer compact code and let the compiler infer the type for a constant or variable.

**Preferred:**
```swift
let message = "Click the button"
var currentBounds = computeViewBounds()
```

**Not Preferred:**
```swift
let message: String = "Click the button"
var currentBounds: CGRect = computeViewBounds()
```

**NOTE**: Following this guideline means picking descriptive names is even more important than before.

### Syntactic Sugar

Prefer the shortcut versions of type declarations over the full generics syntax.

**Preferred:**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Not Preferred:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Functions vs Methods

Free functions, which aren't attached to a class or type, should be used sparingly. When possible, prefer to use a method instead of a free function. This aids in readability and discoverability.

Free functions are most appropriate when they aren't associated with any particular type or instance.

**Preferred**
```swift
let sorted = items.mergeSort()  // easily discoverable
rocket.launch()  // clearly acts on the model
```

**Not Preferred**
```swift
let sorted = mergeSort(items)  // hard to discover
launch(&rocket)
```

**Free Function Exceptions**
```swift
let tuples = zip(a, b)  // feels natural as a free function (symmetry)
let value = max(x,y,z)  // another free function that feels natural
```

## Memory Management

Code (even non-production, tutorial demo code) should not create reference cycles. Analyze your object graph and prevent strong cycles with `weak` and `unowned` references. Alternatively, use value types (`struct`, `enum`) to prevent cycles altogether.

### Extending object lifetime

Extend object lifetime using the `[weak self]` and ``guard let `self` = self else { return }`` idiom. `[weak self]` is preferred to `[unowned self]` where it is not immediately obvious that `self` outlives the closure. Explicitly extending lifetime is preferred to optional unwrapping.

**Preferred**
```swift
resource.request().onComplete { [weak self] response in
    guard let `self` = self else { return }
    let model = self.updateModel(response)
    self.updateUI(model)
}
```

**Not Preferred**
```swift
`// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
    let model = self.updateModel(response)
    self.updateUI(model)
}
```

**Not Preferred**
```swift
`// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
    let model = self?.updateModel(response)
    self?.updateUI(model)
}
```

### Syntactic Sugar

Prefer the shortcut versions of type declarations over the full generics syntax.

**Preferred:**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Not Preferred:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```


## Control Flow

#### Functional constructs

Always prefer functional constructs over `for-in` loops and mutable state.

**Preferred:**
```swift
// Get list of names longer than the number of the child's
// older siblings (Comparable sorts by age)
return parents.flatMap { 
    $0.childen
        .sorted()
        .map { $0.name }
        .enumerated()
        .filter { i, n in n.characters.count > i }
        .map { $1 }
} 
```
**Not Preferred:**
```swift
// Get list of names longer than the number of the child's
// older siblings (Comparable sorts by age)
var results: [String] = []
for parent in parents {
    for index, child in parent.children.sorted().enumerated() {
        let name = child.name
        if name.characters.count > index {
            results.append(name)
        }
    }
}

return results 
```

#### forEach

Prefer `forEach` over `for-in` when applicable.

**Preferred:**

Use named parameters when the object is being referenced more than once.

```swift
attendeeList.forEach { attendee in 
    print("\(attendee.name) is attending with \(attendee.guests.count) guests.") 
}
```

Anonymous parameters

```swift
[subview, anotherSubview].forEach { view.addSubview($0) }
```

There are some disadvantages to using `forEach` over `for-in` which you should probably be
aware of.

```swift
/// - Note: You cannot use the `break` or `continue` statement to exit the
///   current call of the `body` closure or skip subsequent calls.
/// - Note: Using the `return` statement in the `body` closure will only
///   exit from the current call to `body`, not any outer scope, and won't
///   skip subsequent calls.
```

So if the operation demands more control, then use `for-in`.

#### for-in

Prefer the `for-in` style of `for` loop over the `for-condition-increment` style.

**Preferred:**
```swift
for _ in 0..<3 {
    println("Hello three times")
}

for (index, person) in attendeeList.enumerated() {
    println("\(person) is at position #\(index)")
}
```

<!--
**Not Preferred:**
```swift
for var i = 0; i < 3; i++ {
    println("Hello three times")
}

for var i = 0; i < attendeeList.count; i++ {
  let person = attendeeList[i]
  println("\(person) is at position \#\(i)")
}
```
-->

## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

The only exception to this rule is the `for-conditional-increment` construct, which requires semicolons. However, alternative `for-in` constructs should be used where possible.

**Preferred:**
```swift
var swift = "not a scripting language"
```

**Not Preferred:**
```swift
var swift = "not a scripting language";
```

<!--
**NOTE**: Swift is very different to JavaScript, where omitting semicolons is [generally considered unsafe][16]
-->

## Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path. That is, don't nest `if` statements. Multiple return statements are OK. The `guard` statement is built for this.

**Preferred:**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
    guard let context = context else { throw FFTError.NoContext }
    guard let inputData = inputData else { throw FFTError.NoInputData }

    // use context and input to compute the frequencies

    return frequencies
}
```

**Not Preferred:**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
    if let context = context {
        if let inputData = inputData {
            // use context and input to compute the frequencies
            
            return frequencies
        }
        else {
            throw FFTError.NoInputData
        }
    }
    else {
        throw FFTError.NoContext
    }
}
```

When multiple optionals are unwrapped either with `guard` or `if let`, minimize nesting by using the compound version when possible. Example:

**Preferred:**
```swift
guard let number1 = number1, number2 = number2, number3 = number3 else { fatalError("impossible") }
// do something with numbers
```

**Not Preferred:**
```swift
if let number1 = number1 {
    if let number2 = number2 {
        if let number3 = number3 {
            // do something with numbers
        }
        else {
            fatalError("impossible")
        }
    }
    else {
        fatalError("impossible")
    }
}
else {
    fatalError("impossible")
}
```

### Failing Guards

Guard statements are required to exit in some way. Generally, this should be simple one line statement such as `return`, `throw`, `break`, `continue`, and `fatalError()`. Large code blocks should be avoided. If cleanup code is required for multiple exit points, consider using a `defer` block to avoid cleanup code duplication.


## Parenthesis

Parenthesis around conditionals are not required and should be omitted.

**Preferred:**
```swift
if name == "Hello" {
    print("World")
}
```

**Not Preferred:**
```swift
if (name == "Hello") {
    print("World")
}
```

## Resource code

If you have Sketch StyleKit in your project use it for colors. 
else use UIColor extension and name colors with according name 


```swift
extension UIColor {
    
    var defaultBlue : UIColor {
        {
            return UIColor(hex:0x0000FF)
        }
    }

}
```

## Attribution

This document is based on the [raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide) and [Hyper Oslo Swift Style Guide](https://github.com/hyperoslo/iOS-playbook/blob/master/style-guidelines/Swift.md).
