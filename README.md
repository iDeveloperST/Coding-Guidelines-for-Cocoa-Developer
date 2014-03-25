Coding Guidelines for Cocoa Developer
=====================================
<br/>

The purpose of this document is to help the developers how to follow the universal coding guidelines and standards that should be used for cocoa and cocoa touch programming so that will increase the readability and maintainability of our common source base. 


Always follow the golden rule :

> If you are extending, enhancing, or bug fixing already implemented code, use the style that is already being used so that the source is uniform and easy to follow.


## 1) Cocoa Patterns

#### Delegate Pattern
Delegate objects should not be retained when doing so would create a retain cycle.

A class that implements the delegate pattern should typically:

* Have an instance variable named _delegate to reference the delegate.
* Thus, the accessor methods should be named delegate and setDelegate:.
* The _delegate object should be weak if the class is typically retained by its delegate, such that a strong delegate would create a retain cycle.



#### Singletons Pattern

Singleton objects should use a thread-safe pattern for creating their shared instance.

```sh
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
Apple uses this approach a lot. For example: 
```sh
[NSUserDefaults standardUserDefaults]
[UIApplication sharedApplication]
[UIScreen mainScreen]
[NSFileManager defaultManager] 

all return a Singleton object.
```


#### Model/View/Controller Pattern

Separate the model from the view. Separate the controller from the view and the model. Use @protocols for callback APIs.

* Separate model from view: don't build assumptions about the presentation into the model or data source. Keep the interface between the data source and the presentation abstract. Don't give the model knowledge of its view. (A good rule of thumb is to ask yourself if it's possible to have multiple presentations, with different states, on a single instance of your data source.)
* Separate controller from view and model: don't put all of the "business logic" into view-related classes; this makes the code very unusable. Make controller classes to host this code, but ensure that the controller classes don't make too many assumptions about the presentation.
* Define callback APIs with @protocol, using @optional if not all the methods are required.



## 2) Global Symbols

There are several types of symbols with global scope in addition to C functions. For example:

* Constants
* Typedef'd structs
* Typedef'd enums
* Individual enum values
* Objective-C Protocols


### Extern, Const, and Static

```sh
extern NSString *const kMyConstant;
extern NSString *MyExternString;
static NSString *const kMyStaticConstant;
static NSString *staticString;
```

### Naming

In general, everything should be prefixed with a 2-3 letter prefix. Longer prefixes are acceptable, but not recommended.

It is a good idea to prefix classes with an application specific application if it is application specific code. If there is code you plan on using in other applications or open sourcing, it is a good idea to do something specific to your or your company for the prefix.

If you company name is Awesome Buckets and you have an application named Bucket Hunter, here are a few examples:

```sh
ABLoadingView // Simple view that can be used in other applications
BHAppDelegate // Application specific code
BHLoadingView // `ABLoadingView` customized for the Bucket Hunter application
```
### Enums
```sh
enum {
    Foo,
    Bar
};

typedef enum {
    SSLoadingViewStyleLight,
    SSLoadingViewStyleDark
} SSLoadingViewStyle;

typedef enum {
    SSHUDViewStyleLight = 7,
    SSHUDViewStyleDark = 12
} SSHUDViewStyle;
```



## 2) #import and #include

```sh
#import Objective-C/Objective-C++ headers, and #include C/C++ headers.
```
Choose between #import and #include based on the language of the header that you are including.

When including a header that uses Objective-C or Objective-C++, use #import.
When including a standard C or C++ header, use #include. The header should provide its own #define guard.
Some Objective-C headers lack #define guards, and expect to be included only by #import. As Objective-C headers may only be included in Objective-C source files and other Objective-C headers, using #import across the board is appropriate.

This rule helps avoid inadvertent errors in cross-platform projects. A Mac developer introducing a new C or C++ header might forget to add #define guards, which would not cause problems on the Mac if the new header were included with #import, but would break builds on other platforms where #include is used. Being consistent by using #include on all platforms means that compilation is more likely to succeed everywhere or fail everywhere, and avoids the frustration of files working only on some platforms.

```sh
#import <Cocoa/Cocoa.h>
#include <CoreFoundation/CoreFoundation.h>
#import "GTMFoo.h"
#include "base/basictypes.h"
```

<<< About Document >>>
--------

Version
----
1.0

