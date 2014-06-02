Swiftchina 雨燕中文
=======

Swift is an innovative new programming language for Cocoa and Cocoa Touch. Writing code is interactive and fun, the syntax is concise yet expressive, and apps run lightning-fast. Swift is ready for your next iOS and OS X project — or for addition into your current app — because Swift code works side-by-side with Objective-C.

Getting Started
---------------

This guide describes the steps needed to download, install, configure, and run the basic examples for Swift.

##Basic Setup

>IMPORTANT
This is a preliminary document for an API or technology in development. Apple is supplying this information to help you plan for the adoption of the technologies and programming interfaces described herein for use on Apple-branded products. This information is subject to change, and software implemented according to this document should be tested with final operating system software and final documentation. Newer versions of this document may be provided with future seeds of the API or technology.


Swift is designed to provide seamless compatibility with Cocoa and Objective-C. You can use Objective-C APIs (ranging from system frameworks to your own custom code) in Swift, and you can use Swift APIs in Objective-C. This compatibility makes Swift an easy, convenient, and powerful tool to integrate into your Cocoa app development workflow.

This guide covers three important aspects of this compatibility that you can use to your advantage when developing Cocoa apps:

* Interoperability lets you interface between Swift and Objective-C code, allowing you to use Swift classes in Objective-C and to take advantage of familiar Cocoa classes, patterns, and practices when writing Swift code.

* Mix and match allows you to create mixed-language apps containing both Swift and Objective-C files that can communicate with each other.

* Migration from existing Objective-C code to Swift is made easy with interoperability and mix and match, making it possible to replace parts of your Objective-C apps with the latest Swift features.

Before you get started learning about these features, you need a basic understanding of how to set up a Swift environment in which you can access Cocoa system frameworks.

###Setting Up Your Swift Environment

To start experimenting with accessing Cocoa frameworks in Swift, create a Swift-based app from one of the Xcode templates.

**To create a Swift project in Xcode**

1. Choose File > New > Project > (iOS or OS X) > Application > your template of choice.

2. Click the Language pop-up menu and choose Swift.

![](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/Art/newproject_2x.png)

A Swift project’s structure is nearly identical to an Objective-C project, with one important distinction: Swift has no header files. There is no explicit delineation between the implementation and the interface, so all the information about a particular class resides in a single .swift file.

From here, you can start experimenting by writing Swift code in the app delegate, or you can create a new Swift class file by choosing File > New > File > (iOS or OS X) > Other > Swift.

###Understanding the Swift Import Process

After you have your Xcode project set up, you can import any framework from the Cocoa platform to start working with Objective-C from Swift.

Any Objective-C framework (or C library) that’s accessible as a module can be imported directly into Swift. This includes all of the Objective-C system frameworks—such as Foundation, UIKit, and SpriteKit—as well as common C libraries supplied with the system. For example, to import Foundation, simply add this import statement to the top of the Swift file you’re working in:

>import Foundation

This import makes all of the Foundation APIs—including NSDate, NSURL, NSMutableData, and all of their methods, properties, and categories—directly available in Swift.

The import process is straightforward. Objective-C frameworks vend APIs in header files. In Swift, those header files are compiled down to Objective-C modules, which are then imported into Swift as Swift APIs. The importing determines how functions, classes, methods, and types declared in Objective-C code appear in Swift. For functions and methods, this process affects the types of their arguments and return values. For types, the process of importing can do the following things:

* Remap certain Objective-C types to their equivalents in Swift, like id to AnyObject

* Remap certain Objective-C core types to their alternatives in Swift, like NSString to String

* Remap certain Objective-C concepts to matching concepts in Swift, like pointers to optionals

In Interoperability, you’ll learn more about these mappings and about how to leverage them in your Swift code.

The model for importing Swift into Objective-C is similar to the one used for importing Objective-C into Swift. Swift vends its APIs—such as from a framework—as Swift modules. Alongside these Swift modules are generated Objective-C headers. These headers vend the APIs that can be mapped back to Objective-C. Some Swift APIs do not map back to Objective-C because they leverage language features that are not available in Objective-C. For more information on using Swift in Objective-C, see Swift and Objective-C in the Same Project.

>You cannot import C++ code directly into Swift. Instead, create an Objective-C or C wrapper for C++ code.


Interoperability
---------------

##Interacting with Objective-C APIs

Interoperability is the ability to interface between Swift and Objective-C in either direction, letting you access and use pieces of code written in one language in a file of the other language. As you begin to integrate Swift into your app development workflow, it’s a good idea to understand how you can leverage interoperability to redefine, improve, and enhance the way you write Cocoa apps.

One important aspect of interoperability is that it lets you work with Objective-C APIs when writing Swift code. After you import an Objective-C framework, you can instantiate classes from it and interact with them using native Swift syntax.

###Initialization

To instantiate an Objective-C class in Swift, you call one of its initializers with Swift syntax. When Objective-C init methods come over to Swift, they take on native Swift initializer syntax. The “init” prefix gets sliced off and becomes a keyword to indicate that the method is an initializer. For init methods that begin with “initWith,“ the “With” also gets sliced off. The first letter of the selector piece that had “init” or “initWith” split off from it becomes lowercase, and that selector piece is treated as the name of the first argument. The rest of the selector pieces also correspond to argument names. Each selector piece goes inside the parentheses and is required at the call site.

For example, where in Objective-C you would do this:

```
UITableView *myTableView = [[UITableView alloc] initWithFrame:CGRectZero style:UITableViewStyleGrouped];
```

In Swift, you do this:

```
let myTextField = UITextField(frame: CGRect(0.0, 0.0, 200.0, 40.0))

```

These UITableView and UITextField objects have the same familiar functionality that they have in Objective-C. You can use them in the same way you would in Objective-C, accessing any properties and calling any methods defined on the respective classes.

For consistency and simplicity, Objective-C factory methods get mapped as convenience initializers in Swift. This mapping allows them to be used with the same concise, clear syntax as initializers. For example, whereas in Objective-C you would call this factory method like this:

```
UIColor *color = [UIColor colorWithRed:0.5 green:0.0 blue:0.5 alpha:1.0];
```

In Swift, you call it like this:

```
let color = UIColor(red: 0.5, green: 0.0, blue: 0.5, alpha: 1.0)
```

###Accessing Properties

Access and set properties on Objective-C objects in Swift using dot syntax.

```
myTextField.textColor = UIColor.darkGrayColor()
myTextField.text = "Hello world"
if myTextField.editing {
    myTextField.editing = false
}
```

When getting or setting a property, use the name of the property without appending parentheses. Notice that darkGrayColor has a set of parentheses. This is because darkGrayColor is a class method on UIColor, not a property.

In Objective-C, a method that returns a value and takes no arguments can be treated as an implicit getter—and be called using the same syntax as a getter for a property. This is not the case in Swift. In Swift, only properties that are written using the @property syntax in Objective-C are imported as properties. Methods are imported and called as described in Working with Methods.

###Working with Methods

When calling Objective-C methods from Swift, use dot syntax.

When Objective-C methods come over to Swift, the first part of an Objective-C selector becomes the base method name and appears outside the parentheses. The first argument appears immediately inside the parentheses, without a name. The rest of the selector pieces correspond to argument names and go inside the parentheses. All selector pieces are required at the call site.

For example, whereas in Objective-C you would do this:

```
[myTableView insertSubview:mySubview atIndex:2];
```

In Swift, you do this:

```
myTableView.insertSubview(mySubview, atIndex: 2)
```

If you’re calling a method with no arguments, you must still include the parentheses.

```
myTableView.layoutIfNeeded()

```

###id Compatibility

Swift includes a protocol type named AnyObject that represents any kind of object, just as id does in Objective-C. The AnyObject protocol allows you to write type-safe Swift code while maintaining the flexibility of an untyped object. Because of the additional safety provided by the AnyObject protocol, Swift imports id as AnyObject.

For example, as with id, you can assign an object of any class type to a constant or variable typed as AnyObject. You can also reassign a variable to an object of a different type.

```
var myObject: AnyObject = UITableViewCell()
myObject = NSDate()

```

You can also call any Objective-C method and access any property without casting to a more specific class type. This includes Objective-C compatible methods marked with the @objc attribute.

```
let futureDate = myObject.dateByAddingTimeInterval(10)
let timeSinceNow = myObject.timeIntervalSinceNow
```

However, because the specific type of an object typed as AnyObject is not known until runtime, it is possible to inadvertently write unsafe code. Additionally, in contrast with Objective-C, if you invoke a method or access a property that does not exist on an AnyObject typed object, it is a runtime error. For example, the following code compiles without complaint and then causes an unrecognized selector error at runtime:

```
myObject.characterAtIndex(5)
// crash, myObject does't respond to that method
```

However, you can take advantage of optionals in Swift to eliminate this common Objective-C error from your code. When you call an Objective-C method on an AnyObject type object, the method call actually behaves like an implicitly unwrapped optional. You can use the same optional chaining syntax you would use for optional methods in protocols to optionally invoke a method on AnyObject. This same process applies to properties as well.

For example, in the code listing below, the first and second lines are not executed because the length property and the characterAtIndex: method do not exist on an NSDate object. The myLength constant is inferred to be an optional Int, and is set to nil. You can also use an if–let statement to conditionally unwrap the result of a method that the object may not respond to, as shown on line three.

```
let myLength = myObject.length?
let myChar = myObject.characterAtIndex?(5)
if let fifthCharacter = myObject.characterAtIndex(5) {
    println("Found \(fifthCharacter) at index 5")
}
```





