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

As with all downcasts in Swift, casting from AnyObject to a more specific object type is not guaranteed to succeed and therefore returns an optional value. You can check that optional value to determine whether the cast succeeded.

```
let userDefaults = NSUserDefaults.standardUserDefaults()
let lastRefreshDate: AnyObject? = userDefaults.objectForKey("LastRefreshDate")
if let date = lastRefreshDate as? NSDate {
    println("\(date.timeIntervalSinceReferenceDate)")
}
```

Of course, if you are certain of the type of the object (and know that it is not nil), you can force the invocation with the as operator.

```
let myDate = lastRefreshDate as NSDate
let timeInterval = myDate.timeIntervalSinceReferenceDate

```

###Working with nil

In Objective-C, you work with references to objects using raw pointers that could be NULL (also referred to as nil in Objective-C). In Swift, all values—including structures and object references—are guaranteed to be non–nil. Instead, you represent a value that could be missing by wrapping the type of the value in an optional type. When you need to indicate that a value is missing, you use the value nil. You can read more about optionals in Optionals.

Because Objective-C does not make any guarantees that an object is non-nil, Swift makes all classes in argument types and return types optional in imported Objective-C APIs. Before you use an Objective-C object, you should check to ensure that it is not missing.

In some cases, you might be absolutely certain that an Objective-C method or property never returns a nil object reference. To make objects in this special scenario more convenient to work with, Swift imports object types as implicitly unwrapped optionals. Implicitly unwrapped optional types include all of the safety features of optional types. In addition, you can access the value directly without checking for nil or unwrapping it yourself. When you access the value in this kind of optional type without safely unwrapping it first, the implicitly unwrapped optional checks whether the value is missing. If the value is missing, a runtime error occurs. As a result, you should always check and unwrap an implicitly unwrapped optional yourself, unless you are sure that the value cannot be missing.


###Extensions

A Swift extension is similar to an Objective-C category. Extensions expand the behavior of existing classes, structures, and enumerations, including those defined in Objective-C. You can define an extension on a type from either a system framework or one of your own custom types. Simply import the appropriate module, and refer to the class, structure, or enumeration by the same name that you would use in Objective-C.

For example, you can extend the UIBezierPath class to create a simple Bézier path with an equilateral triangle, based on a provided side length and starting point.

```
extension UIBezierPath {
    convenience init(triangleSideLength: Float, origin: CGPoint) {
        self.init()
        let squareRoot = Float(sqrt(3))
        let altitude = (squareRoot * triangleSideLength) / 2
        moveToPoint(origin)
        addLineToPoint(CGPoint(triangleSideLength, origin.x))
        addLineToPoint(CGPoint(triangleSideLength / 2, altitude))
        closePath()
    }
}
```

You can use extensions to add properties (including class and static properties). However, these properties must be computed; extensions can’t add stored properties to classes, structures, or enumerations.

This example extends the CGRect structure to contain a computed area property:

```
extension CGRect {
    var area: CGFloat {
    return width * height
    }
}
let rect = CGRect(x: 0.0, y: 0.0, width: 10.0, height: 50.0)
let area = rect.area
// area: CGFloat = 500.0
```

You can also use extensions to add protocol conformance to a class without subclassing it. If the protocol is defined in Swift, you can also add conformance to it to structures or enumerations, whether defined in Swift or Objective-C.

You cannot use extensions to override existing methods or properties on Objective-C types.

###Closures

Objective-C blocks are automatically imported as Swift closures. For example, here is an Objective-C block variable:

```
void (^completionBlock)(NSData *, NSError *) = ^(NSData *data, NSError *error) {/* ... */}
```

And here’s what it looks like in Swift:

```
let completionBlock: (NSData, NSError) -> Void = {data, error in /* ... */}

```

Swift closures and Objective-C blocks are compatible, so you can pass Swift closures to Objective-C methods that expect blocks. Swift closures and functions have the same type, so you can even pass the name of a Swift function.

Closures have similar capture semantics as blocks but differ in one key way: Variables are mutable rather than copied. In other words, the behavior of __block in Objective-C is the default behavior for variables in Swift.

###Object Comparison

There are two distinct types of comparison when you compare two objects in Swift. The first, equality (==), compares the contents of the objects. The second, identity (===), determines whether or not the constants or variables refer to the same object instance.

Swift and Objective-C objects are typically compared in Swift using the == and === operators. Swift provides a default implementation of the == operator for objects that derive from the NSObject class. In the implementation of this operator, Swift invokes the isEqual: method defined on the NSObject class. The NSObject class only performs an identity comparison, so you should implement your own isEqual: method in classes that derive from the NSObject class. Because you can pass Swift objects (including ones not derived from NSObject) to Objective-C APIs, you should implement the isEqual: method for these classes if you want the Objective-C APIs to compare the contents of the objects rather than their identities.

As part of implementing equality for your class, be sure to implement the hash property according to the rules in Object comparison. Further, if you want to use your class as keys in a dictionary, also conform to the Hashable protocol and implement the hashValue property.


###Swift Type Compatibility

When you define a Swift class that inherits from NSObject or any other Objective-C class, the class is automatically compatible with Objective-C. All of the steps in this section have already been done for you by the Swift compiler. If you never import a Swift class in Objective-C code, you don’t need to worry about type compatibility in this case as well. Otherwise, if your Swift class does not derive from an Objective-C class and you want to use it from Objective-C code, you can use the @objc attribute described below.

The @objc attribute makes your Swift API available in Objective-C and the Objective-C runtime. In other words, you can use the @objc attribute before any Swift method, property, or class that you want to use from Objective-C code. If your class inherits from an Objective-C class, the compiler inserts the attribute for you. The compiler also adds the attribute to every method and property in a class that is itself marked with the @objc attribute. When you use the @IBOutlet, @IBAction, or @NSManaged attribute, the @objc attribute is added as well. This attribute is also useful when you’re working with Objective-C classes that use selectors to implement the target-action design pattern—for example, NSTimer or UIButton.

When you use a Swift API from Objective-C, the compiler typically performs a direct translation. For example, the Swift API func playSong(name: String) is imported as - (void)playSong:(NSString *)name in Objective-C. However, there is one exception: When you use a Swift initializer in Objective-C, the compiler adds the text “initWith” to the beginning of the method and properly capitalizes the first character in the original initializer. For example, this Swift initializer init (songName: String, artist: String) is imported as - (instancetype)initWithSongName:(NSString *)songName artist:(NSString *)artist in Objective-C.

Swift also provides a variant of the @objc attribute that allows you to specify name for your symbol in Objective-C. For example, if the name of your Swift class contains a character that isn’t supported by Objective-C, you can provide an alternative name to use in Objective-C. If you provide an Objective-C name for a Swift function, use Objective-C selector syntax. Remember to add a colon (:) wherever a parameter follows a selector piece.

```
@objc(Squirrel)
class Белка {
    @objc(initWithName:)
    init (имя: String) { /*...*/ }
    @objc(hideNuts:inTree:)
    func прячьОрехи(Int, вДереве: Дерево) { /*...*/ }
}
```

When you use the @objc(<#name#>) attribute on a Swift class, the class is made available in Objective-C without any namespacing. As a result, this attribute can also be useful when you migrate an archivable Objective-C class to Swift. Because archived objects store the name of their class in the archive, you should use the @objc(<#name#>) attribute to specify the same name as your Objective-C class so that older archives can be unarchived by your new Swift class.

###Objective-C Selectors

An Objective-C selector is a type that refers to the name of an Objective-C method. In Swift, Objective-C selectors are represented by the Selector structure. You can construct a selector with a string literal, such as let mySelector: Selector = "tappedButton:". Because string literals can be automatically converted to selectors, you can pass a string literal to any method that accepts a selector.

```
import UIKit
class MyViewController: UIViewController {
    let myButton = UIButton(frame: CGRect(x: 0, y: 0, width: 100, height: 50))

    init(nibName nibNameOrNil: String!, bundle nibBundleOrNil: NSBundle!) {
        super.init(nibName: nibName, bundle: nibBundle)
        myButton.targetForAction("tappedButton:", withSender: self)
    }

    func tappedButton(sender: UIButton!) {
        println("tapped button")
    }
}
```

>The performSelector: method and related selector-invoking methods are not imported in Swift because they are inherently unsafe.

If your Swift class inherits from an Objective-C class, all of the methods and properties in the class are available as Objective-C selectors. Otherwise, if your Swift class does not inherit from an Objective-C class, you need to prefix the symbol you want to use as a selector with the @objc attribute, as described in Swift Type Compatibility.

##Writing Swift Classes with Objective-C Behavior

Interoperability lets you define Swift classes that incorporate Objective-C behavior. You can subclass Objective-C classes, adopt Objective-C protocols, and take advantage of other Objective-C functionality when writing a Swift class. This means that you can create classes based on familiar, established behavior in Objective-C and enhance them with Swift’s modern and powerful language features.

###Inheriting from Objective-C Classes

In Swift, you can define subclasses of Objective-C classes. To create a Swift class that inherits from an Objective-C class, add a colon (:) after the name of the Swift class, followed by the name of the Objective-C class.

```
import UIKit

class MySwiftViewController: UIViewController {
    // define the class
}
```

You get all the functionality offered by the superclass in Objective-C. If you provide your own implementations of the superclass’s methods, remember to use the override keyword.

###Adopting Protocols

In Swift, you can adopt protocols that are defined in Objective-C. Like Swift protocols, any Objective-C protocols go in a comma-separated list following the name of a class’s superclass, if any.

```
class MySwiftViewController: UIViewController, UITableViewDelegate, UITableViewDataSource {
    // define the class
}
```

Objective-C protocols come in as Swift protocols. If you want to refer to the UITableViewDelegate protocol in Swift code, refer to it as UITableViewDelegate (as compared to id<UITableViewDelegate> in Objective-C).

Because the namespace of classes and protocols is unified in Swift, the NSObject protocol in Objective-C is remapped to NSObjectProtocol in Swift.


###Writing Initializers and Deinitializers

The Swift compiler ensures that your initializers do not leave any properties in your class uninitialized to increase the safety and predictability of your code. Additionally, unlike Objective-C, in Swift there is no separate memory allocation method to invoke. You use native Swift initialization syntax even when you are working with Objective-C classes—Swift converts Objective-C initialization methods to Swift initializers. You can read more about implementing your own initializers in Initializers.

When you want to perform additional clean-up before your class is deallocated, you can implement a deninitializer instead of the dealloc method. Swift deinitializers are called automatically, just before instance deallocation happens. Swift automatically calls the superclass deinitializer after invoking your subclass’s deinitializer. When you are working with an Objective-C class or your Swift class inherits from an Objective-C class, Swift calls your class’s superclass dealloc method for you as well. You can read more about implementing your own deinitializers in Deinitializers.

###Integrating with Interface Builder

The Swift compiler includes attributes that enable Interface Builder features for your Swift classes. As in Objective-C, you can use outlets, actions, and live rendering in Swift.


###Working with Outlets and Actions

Outlets and actions allow you to connect your source code to user interface objects in Interface Builder. To use outlets and actions in Swift, insert @IBOutlet or @IBAction just before the property or method declaration. You use the same @IBOutlet attribute to declare an outlet collection—just specify an array for the type.

When you declare an outlet in Swift, the compiler automatically converts the type to a weak implicitly unwrapped optional and assigns it an initial value of nil. In effect, the compiler replaces @IBOutlet var name: Type with @IBOutlet weak var name: Type! = nil. The compiler converts the type to an implicitly unwrapped optional so that you aren’t required to assign a value in an initializer. It is implicitly unwrapped because after your class is initialized from a storyboard or xib file, you can assume that the outlet has been connected. Outlets are weak by default because the outlets you create usually have weak relationships.

For example, the following Swift code declares a class with an outlet, an outlet collection, and an action:

```
class MyViewController: UIViewController {
    @IBOutlet var button: UIButton
    @IBOutlet var textFields: UITextField[]
    @IBAction func buttonTapped(AnyObject) {
        println("button tapped!")
    }
}
```

Because the sender parameter of the buttonTapped: method wasn’t used, the parameter name can be omitted.

###Live Rendering

You can use two different attributes—@IBDesignable and @IBInspectable—to enable live, interactive custom view design in Interface Builder. When you create a custom view that inherits from UIView or NSView, you can add the @IBDesignable attribute just before the class declaration. After you add the custom view to Interface Builder (by setting the custom class of the view in the inspector pane), Interface Builder renders your view in the canvas.

>Live rendering can be used only from frameworks.

You can also add the @IBInspectable attribute to properties with types compatible with user defined runtime attributes. After you add your custom view to Interface Builder, you can edit these properties in the inspector.

```
@IBDesignable
class MyCustomView: UIView {
    @IBInspectable var textColor: UIColor
    @IBInspectable var iconHeight: CGFloat
    /* ... */
}
```

###Specifying Property Attributes

In Objective-C, properties have a range of potential attributes that specify additional information about a property’s behavior. In Swift, you specify these property attributes in a different way.

###Strong and Weak

Swift properties are strong by default. Use the weak keyword to indicate that a property has a weak reference to the object stored as its value. This keyword can be used only for properties that are optional class types. For more information, see Attributes.

###Read/Write and Read-Only

In Swift, there are no readwrite and readonly attributes. When declaring a stored property, use let to make it read-only, and use var to make it read/write. When declaring a computed property, provide a getter only to make it read-only and provide both a getter and setter to make it read/write. For more information, see Properties.

###Copy Semantics

In Swift, the Objective-C copy property attribute translates to @NSCopying. The type of the property must conform to the NSCopying protocol. For more information, see Attributes.

###Implementing Core Data Managed Object Subclasses

Core Data provides the underlying storage and implementation of properties in subclasses of the NSManagedObject class. Add the @NSManaged attribute before each property definition in your managed object subclass that corresponds to an attribute or relationship in your Core Data model. Like the @dynamic attribute in Objective-C, the @NSManaged attribute informs the Swift compiler that the storage and implementation of a property will be provided at runtime. However, unlike @dynamic, the @NSManaged attribute is available only for Core Data support.




















