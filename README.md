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
