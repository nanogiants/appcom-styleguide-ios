# Appcom iOS Style Guide

This document describes the style guide applied to iOS projects for appcom interactive GmbH. It describes rules how to organize your project, dependencies and files, so that some best practises are held. It is based on the [Appcom General Code Style Guide](http://appcom-bitbucket/projects/APCON/repos/appcom-conventions-general) which should be known to understand all parts of this guide.

So use it in your favor if you want to and/or override the style guide in any way you want.

## Table of Contents

<a name="table-of-contents"></a>

1. [Xcode](#xcode)
1. [Artifacts](#artifacts)
1. [Dependencies](#dependencies)
1. [Comments / Doxygen](#comments)
1. [File Names](#file-names)
1. [Objective-C](#objc)
1. [Asset Naming Conventions](#asset-naming-conventions)
1. [Logging](#logging)
1. [Roadmap](#roadmap)
1. [Resources](#resources)
1. [License](#license)

## Xcode

<a name="xcode"></a>

### Xcode version

<a name="xcode-version"></a>

[1.1](#xcode-version)
The used Xcode version must always be the latest available stable version if possible. You must not use a beta version since this can break the continuous integration system.

### Xcode class prefix

<a name="xcode-class-prefix"></a>

[1.2](#xcode-class-prefix)
In the Project the Organization must be set as `appcom interactive GmbH` and the Class-Prefix `AC`.

### Xcode structure

<a name="xcode-structure"></a>

[1.3](#xcode-structure)
Project structure is important since it helps when navigating through the code and has a huge impact of the overall maintainability of the project. Each group that contains files must refer to a corresponding folder if possible, so that the files on the harddisk are organized as well. Since iOS Projects follow the MVC-Pattern, the folder structure should represent this and keep this Pattern straight forward. The following table will list all folders that must be present in the project:

| Folder / Group | Description |
|---|---|
| Models | Contains all classes that correspond to the Model in the MVC-Pattern. |
| Views | Contains all classes that correspond to the View in the MVC-Pattern. |
| Controllers | Contains all classes that correspond to the Controller in the MVC-Pattern. |
| Categories | Contains all classes that extend functionalities of well known classes. |
| Storyboards | Contains all storyboards and xib files. |
| Resources | Contains data that is delivered with the app like localization, fonts and databases. |


### Xcode targets

<a name="xcode-targets"></a>

[1.4](#xcode-targets)
A Xcode-Project contains at least one target. Depending on the project there can be multiple shared targets. In many cases there are 3 targets that represent the following environments: develop, staging and  production. 
If there are changes applied to one of the targets, the other targets must be considered for these changes too. If for example classes are added, these classes are most likely needed by the other targets too. At the end, if the implementation work is done, all targets have to at least compile and run. 

**[back to top](#table-of-contents)**

## Artifacts

<a name="artifacts"></a>

### Artifact name

<a name="artifacts-name"></a>

[2.1](#artifacts-name)
The artifact must be named after the following scheme:

```
$COMPANYNAME$-$APPNAME$-$STAGE$-$VERSION$.$BUILDNUMBER$.ipa

// good
appcom-swipe-production-0.0.2.127.ipa
appcom-swipe-develop-0.0.2.20.ipa

// bad
appcom-swipe.ipa
swipe-0.0.1.ipa
```

**[back to top](#table-of-contents)**

## Dependencies

<a name="dependencies"></a>

### Library version

<a name="dependencies-library-version"></a>

[3.1](#dependencies-library-version)
Always use the latest stable version of a library if possible.
Make sure to migrate also major releases if possible.

### Library provision

<a name="dependencies-library-provision"></a>

[3.2](#dependencies-library-provision)
Libraries must be included using git submodules if compiled within the project itself or using an artefact repository (like sonatype nexus) if precompiled. Provide a script for installing these if needed. The name of the script must be install-deps.sh and placed at the source root.
Don't use any dependency managers like Cocoapods.

### Folder

<a name="dependencies-folder"></a>

[3.3](#dependencies-folder)
Dependencies should be found and structured at a well known place. Therefore these must be placed under the `deps` folder at the souce root. Precompiled libraries must be placed under the subfolder `lib` and the corresponding header files must be under the subfolder `include`. In the Xcode-Project the `Header Search Path` must contain the entry `$(SRCROOT)/deps/include` and the `Library Search Path` must contain `$(SRCROOT)/deps/lib`.

**[back to top](#table-of-contents)**

## Comments / Doxygen

<a name="comments"></a>

### Documentation

<a name="comments-documentation"></a>

[4.1](#comments-documentation)
Every class and it's methods must contain a description of what it is for and how it does it's task. To utilize Xcodes doxygen support and to be able to generate a documentation, these descriptions must use the doxygen syntax. To be uniformly the `!` is used to mark a doxygen comment and `@` to mark commands like `@brief` and `@param`.

```
// good

/*!
* @brief This does something.
* @details A more detailed description ...
* @param tag The tag that is used to do something.
* @return YES if done successfully, NO on failure.
* @sa doSomethingRelated:tag:tag2
*/
- (BOOL)doSomething:(NSString *)tag;

// bad

/**
* \brief This does something.
* \param tag The tag that is used to do something.
*/
- (void)doSomething:(NSString *)tag;
```

### Multiline comments

<a name="comments-multiline-comments"></a>

[4.2](#comments-multiline-comments)
Use `/*! ... */` for block comments.

```
// good

/*!
* @brief This does something.
* @param tag The tag.
*/
- (BOOL)doSomething:(NSString *)tag;

// bad

// @brief This does something.
// @param tag The tag.
- (BOOL)doSomething:(NSString *)tag;
```

### Singleline comment

<a name="comments-singleline-comment"></a>

[4.3](#comments-singleline-comment)
Use `//` for single line comments.
Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it's on the first line of a block.

```
// good
// is current tab
int active = true;

// bad
int active = true; // is current tab


// good
- (int) getType() 
{
    os_log_debug(OS_LOG_DEFAULT, "%s(%i): Fetching type...", __FILE__, __LINE__);

    // set the default type to 'no type'
    int typeOut = self.type ? self.type : -1;

    return typeOut;
}

// bad
- (int) getType() 
{
    os_log_debug(OS_LOG_DEFAULT, "%s(%i): Fetching type...", __FILE__, __LINE__);
    // set the default type to 'no type'
    int typeOut = self.type ? self.type : -1;
    
    return typeOut;
}


// also good
- (int) getType() 
{
    // set the default type to 'no type'
    int typeOut = self.type ? self.type : -1;
    
    return type;
}
```

### Spaces

<a name="comments-spaces"></a>

[4.4](#comments-spaces)
Start all comments with a space to make it easier to read.

```
// good
// is current tab
int active = true;

// bad
//is current tab
int active = true;
```

### Fixme

<a name="comments-fixme"></a>

[4.5](#comments-fixme)
Use `// FIXME:` to annotate problems.

```
@implementation Calculator

+ (instancetype) init
{
    self = [super init];
    if (self) 
    {
        // FIXME: shouldn't use a global here
        _total = 0;
    }
    return self;
}
@end
```

### Todo

<a name="comments-todo"></a>

[4.6](#comments-todo)
Use `// TODO:` to annotate solutions to problems.

```
@implementation Calculator

+ (instancetype) init
{
    self = [super init];
    if (self) 
    {
        // TODO: total should be configurable by an options param
        _total = 0;
    }
    return self;
}

@end
```

## File Names

<a name="file-names"></a>

[5](#file-names)
File names must reflect the name of the class implementation that they contain—including case.
Follow the convention that your project uses. File extensions must be as follows:

| Extension | Type |
|---|---|
| .h | C/Objective-C/Objective-C++ header file |
| .hpp | C++ header file |
| .m | Objective-C implementation file |
| .mm | Objective-C++ implementation file |
| .cpp | Pure C++ implementation file |
| .c | C implementation file |
| .swift | Swift implementation file |

Files containing code that may be shared across projects or used in a large project must have a clearly unique name, typically including the project or class prefix.
File names for categories must include the name of the class being extended, like ACNSString+Utils.h or NSTextView+ACAutocomplete.h

**[back to top](#table-of-contents)**

## Objective-C

<a name="objc"></a>

### Files

<a name="objc-files"></a>

#### File name

<a name="objc-files-file-name"></a>

[6.1.1](#objc-files-file-name)
The source file name consists of the case-sensitive name of the top-level class it contains plus the `.m` or `.mm` extension.

#### File encoding: UTF-8

<a name="objc-files-file-encoding"></a>

[6.1.2](#objc-files-file-encoding)
Source files are encoded in UTF-8.

#### Whitespace characters

<a name="objc-files-whitespace-characters"></a>

[6.1.3](#objc-files-whitespace-characters)
Aside from the line terminator sequence, the ASCII horizontal space character (0x20) is the only whitespace character that appears anywhere in a source file.

This implies that:

* All other whitespace characters in string and character literals are escaped.
* Tab characters are not used for indentation.

#### Header file structure

<a name="objc-files-header-file-structure"></a>

[6.1.4](#objc-files-header-file-structure)
A header file consists of, *in order*:

* License or copyright information, if present
* Import statements
* Forward declarations
* Protocol definitions
* At most one top-level class

#### Source file structure

<a name="objc-files-source-file-structure"></a>

[6.1.5](#objc-files-source-file-structure)
A source file consists of, *in order*:

* License or copyright information, if present
* Import statements
* Exactly one top-level class

**[back to top](#table-of-contents)**

### Formatting

<a name="objc-formatting"></a>

#### Braces

<a name="objc-formatting-braces"></a>

##### Braces are used where optional

<a name="objc-formatting-braces-used"></a>

[6.2.1](#objc-formatting-braces-used)
Braces are used with `if`, `else`, `for`, `do` and `while` statements, even when the body is empty or contains only a single statement.

##### Nonempty blocks

<a name="objc-formatting-braces-nonempty-blocks"></a>

[6.2.2](#objc-formatting-braces-nonempty-blocks)
Braces have the following rules for nonempty blocks and block-like constructs:

* Line break before the opening brace.
* Line break after the opening brace.
* Line break before the closing brace.
* Line break after the closing brace, only if that brace terminates a statement or terminates the body of a method, constructor, or named class. For example, there is no line break after the brace if it is followed by a closing square bracket ( ] ) or a comma.

```
// good
+ (instancetype) init
{
    self = [super init];
    if (self) 
    {
        // ...
    }
    return self;
}

// good
[alert addAction:[UIAlertAction actionWithTitle:@"OK"
                                          style:UIAlertActionStyleDefault
                                        handler:^(UIAlertAction *action)
{
  // ...
}]];

// bad
+ (instancetype) init {
    // ...
}

// bad
if (something) {
    // ...
}
```

##### Empty blocks: may be concise

<a name="objc-formatting-braces-empty-blocks"></a>

[6.2.3](#objc-formatting-braces-empty-blocks)
An empty block or block-like construct may be closed immediately after it is opened, with no characters or line break in between (`{}`), unless it is part of a multi-block statement (one that directly contains multiple blocks: `if/else` or `try/catch/finally`).

```
// This is acceptable
void doNothing() {}

// This is equally acceptable
void doNothingElse()
{
}
```

#### Spaces vs. Tabs

Use only spaces, and indent 4 spaces at a time. We use spaces for indentation. Do not use tabs in your code.
You should set your editor to emit spaces when you hit the tab key, and to trim trailing spaces on lines.

#### Line Length

The maximum line length for Objective-C files is 120 columns.
You can make violations easier to spot by enabling Preferences > Text Editing > Page guide at column: 120 in Xcode.

#### Method Declarations and Definitions

One space must be used between the - or + and the return type, and no spacing in the parameter list except between parameters.

```
// good
- (void)doSomethingWithString:(NSString *)theString 
{
    // ...
}
```

The spacing before the asterisk is optional. When adding new code, be consistent with the surrounding file’s style.
If you have too many parameters to fit on one line, giving each its own line is preferred. If multiple lines are used, align each using the colon before the parameter.

```
// good
- (void)doSomethingWithFoo:(ACFoo *)theFoo
                      rect:(NSRect)theRect
                  interval:(float)theInterval 
{
    // ...
}
```

When the second or later parameter name is longer than the first, indent the second and later lines by at least four spaces, maintaining colon alignment:

```
// good
- (void)short:(ACFoo *)theFoo
          longKeyword:(NSRect)theRect
    evenLongerKeyword:(float)theInterval
                error:(NSError **)theError 
{
    // ...
}
```

#### One statement per line

Each statement is followed by a line break.

#### Conditionals

Include a space after `if`, `while`, `for`, and `switch`, and around comparison operators.

```
// good
for (int i = 0; i < 5; ++i) 
{
}
```

Intentional fall-through to the next case must be documented with a comment unless the case has no intervening code before the next case.

```
// good
switch (i) {
    case 1:
        // ...
        break;
    case 2:
        j++;
        // falls through
    case 3: 
    {
        int k;
        // ...
        break;
    }
    case 4:
    case 5:
    case 6: 
    default: break;
}
```

#### Expressions

Use a space around binary operators and assignments. Omit a space for a unary operator. Do not add spaces inside parentheses.

```
// good
x = 0;
v = w * x + y / z;
v = -y * (x + z);

// bad
v = -y * ( x + z );
v = x * ( x+z );
```

Factors in an expression may omit spaces.

```
// good
v = w*x + y/z;

// bad
v = w*x+y/z;

// bad
v = w * x+y / z;
```

#### Method Invocations

Method invocations must be formatted much like method declarations.
When there’s a choice of formatting styles, follow the convention already used in a given source file. Invocations must have all arguments on one line:

```
// good
[myObject doFooWith:arg1 name:arg2 error:arg3];
```

or have one argument per line, with colons aligned:

```
// good
[myObject doFooWith:arg1
               name:arg2
              error:arg3];
```

Don’t use any of these styles:

```
// bad
[myObject doFooWith:arg1 name:arg2  // some lines with >1 arg
              error:arg3];

// bad
[myObject doFooWith:arg1
               name:arg2 error:arg3];

// bad
[myObject doFooWith:arg1
          name:arg2  // aligning keywords instead of colons
          error:arg3];
```

As with declarations and definitions, when the first keyword is shorter than the others, indent the later lines by at least four spaces, maintaining colon alignment:

```
// good
[myObj short:arg1
          longKeyword:arg2
    evenLongerKeyword:arg3
                error:arg4];
```

Invocations containing multiple inlined blocks may have their parameter names left-aligned at a four space indent.

#### Function Calls

Function calls must include as many parameters as fit on each line, except where shorter lines are needed for clarity or documentation of the parameters.
Continuation lines for function parameters must be indented to align with the opening parenthesis.
If this conflicts with the [Line Length Limit](#line-length), it must have a four-space indent.

```
// good
CFArrayRef array = CFArrayCreate(kCFAllocatorDefault, objects, numberOfObjects,
                                 &kCFTypeArrayCallBacks);

NSString *string = NSLocalizedStringWithDefaultValue(@"FEET", @"DistanceTable",
    resourceBundle,  @"%@ feet", @"Distance for multiple feet");

// Score heuristic.
UpdateTally(scores[x] * y + bases[x],
            x, y, z);

TransformImage(image,
               x1, x2, x3,
               y1, y2, y3,
               z1, z2, z3);
```

Use local variables with descriptive names to shorten function calls and reduce nesting of calls.

```
// good
double scoreHeuristic = scores[x] * y + bases[x];
UpdateTally(scoreHeuristic, x, y, z);
```

**[back to top](#table-of-contents)**

### Classes

Add always a description method for entities and models. Why: You almost always have the need to log the current state of a pojo. And that is when you release you get something like this `<ACMyClass: 0x170010840>`. So keep yourself from this and create a `- (NSString *)description`-Method.

**[back to top](#table-of-contents)**

### Naming Conventions

Names must be as descriptive as possible, within reason. Follow standard [Objective-C naming rules](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html).
Avoid non-standard abbreviations. Don’t worry about saving horizontal space as it is far more important to make your code immediately understandable by a new reader. For example:

```
// good

int numberOfErrors = 0;
int completedConnectionsCount = 0;
tickets = [[NSMutableArray alloc] init];
userInfo = [someObject object];
port = [network port];
NSDate *gAppLaunchDate;

// bad

int w;
int nerr;
int nCompConns;
tix = [[NSMutableArray alloc] init];
obj = [someObject object];
p = [network port];
```

#### Use camelCase when naming objects, functions and instances.

```
// good
int test = 0;
NSString* firstName = "Hans";

// bad
int TEst = 0;
NSString* first_name = "Hans";
```

#### Use PascalCase only when naming classes.

```
// good
@interface FileHandler
    // ...
@end

// bad
@interface fileHandler
    // ...
@end
```

#### Acronyms and initialisms

Acronyms and initialisms must always follow the camelCase rule.

```
// good
@interface SmsContainer
    // ...
@end

// good
@interface HttpRequest
    // ...
@end

// bad
@interface SMSContainer
    // ...
@end

// bad
@interface HTTPRequest
    // ...
@end
```

#### Variable Names

Variable names typically start with a lowercase and use mixed case to delimit words.

Instance variables have leading underscores. File scope or global variables have a prefix g and constants have a prefix k.
For example: `myLocalVariable`, `_myInstanceVariable`, `gMyGlobalVariable`, `kMyConstant`.


**[back to top](#table-of-contents)**

### Avoid Throwing Exceptions

Don’t `@throw` Objective-C exceptions, but you must be prepared to catch them from third-party or OS calls.

This follows the recommendation to use error objects for error delivery in [Apple’s Introduction to Exception Programming Topics for Cocoa](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Exceptions/Exceptions.html).

Use of `@try`, `@catch`, and `@finally` are allowed when required to properly use 3rd party code or libraries. If you do use them, please document exactly which methods you expect to throw.

### Avoid +new

Do not invoke the `NSObject` class method `new`, nor override it in a subclass. Instead, use `alloc` and `init` methods to instantiate retained objects.

Modern Objective-C code explicitly calls `alloc` and an `init` method to create and retain an object. As the `new` class method is rarely used, it makes reviewing code for correct memory management more difficult.

### Keep the Public API Simple

Keep your class simple; avoid “kitchen-sink” APIs. If a method doesn’t need to be public, keep it out of the public interface.

Unlike C++, Objective-C doesn’t differentiate between public and private methods; any message may be sent to an object. As a result, avoid placing methods in the public API unless they are actually expected to be used by a consumer of the class. This helps reduce the likelihood they’ll be called when you’re not expecting it. This includes methods that are being overridden from the parent class.

Since internal methods are not really private, it’s easy to accidentally override a superclass’s “private” method, thus making a very difficult bug to squash. In general, private methods should have a fairly unique name that will prevent subclasses from unintentionally overriding them.

### #import and #include

`#import` Objective-C and Objective-C++ headers, and `#include` C/C++ headers.
Choose between `#import` and `#include` based on the language of the header that you are including.


### Order of Includes

The standard order for header inclusion is the related header, operating system headers, language library headers, and finally groups of headers for other dependencies.

The related header precedes others to ensure it has no hidden dependencies. For implementation files the related header is the header file. For test files the related header is the header containing the tested interface.

A blank line may separate logically distinct groups of included headers.

Import headers using their path relative to the project’s source directory.

```
// good
#import "ProjectX/BazViewController.h"

#import <Foundation/Foundation.h>

#include <unistd.h>
#include <vector>

#include "base/basictypes.h"
#include "base/integral_types.h"
#include "util/math/mathutil.h"

#import "ProjectX/BazModel.h"
#import "Shared/Util/Foo.h"
```

**[back to top](#table-of-contents)**

### Macros

Avoid macros, especially where const variables, enums, XCode snippets, or C functions may be used instead.

Macros make the code you see different from the code the compiler sees. Modern C renders traditional uses of macros for constants and utility functions unnecessary. Macros must only be used when there is no other solution available.

Where a macro is needed, use a unique name to avoid the risk of a symbol collision in the compilation unit. If practical, keep the scope limited by `#undefining` the macro after its use.

Macro names must use `SHOUTY_SNAKE_CASE`—all uppercase letters with underscores between words. Function-like macros may use C function naming practices. Do not define macros that appear to be C or Objective-C keywords.

```
// good
#define AC_EXPERIMENTAL_BUILD ...

// good, macro style
// Assert unless X > Y
#define AC_ASSERT_GT(X, Y) ...

// good, function style.
// Assert unless X > Y
#define ACAssertGreaterThan(X, Y) ...

// bad
#define kIsExperimentalBuild ...

// bad
#define unless(X) if(!(X))
```

Avoid macros that expand to unbalanced C or Objective-C constructs. Avoid macros that introduce scope, or may obscure the capturing of values in blocks.

Avoid macros that generate class, property, or method definitions in headers to be used as public API. These only make the code hard to understand, and the language already has better ways of doing this.

Avoid macros that generate method implementations, or that generate declarations of variables that are later used outside of the macro. Macros must not make code hard to understand by hiding where and how a variable is declared.

```
// bad
#define ARRAY_ADDER(CLASS) \
  -(void)add ## CLASS ## :(CLASS *)obj toArray:(NSMutableArray *)array

// bad - where is 'array' defined?
ARRAY_ADDER(NSString) \
{
  if (array.count > 5) \
  {
    // ...
  }
}
```

Examples of acceptable macro use include assertion and debug logging macros that are conditionally compiled based on build settings—often, these are not compiled into release builds.

**[back to top](#table-of-contents)**

### Nonstandard Extensions

Nonstandard extensions to C/Objective-C may not be used unless otherwise specified.

Compilers support various extensions that are not part of standard C. Examples include compound statement expressions (e.g. foo = ({ int x; Bar(&x); x })) and variable-length arrays.

`__attribute__` is an approved exception, as it is used in Objective-C API specifications.

The binary form of the conditional operator, A ?: B, is an approved exception.

**[back to top](#table-of-contents)**

## Asset Naming Conventions

### Image Naming

Images must be named after the following pattern:

```
<WHAT>_<WHERE>_<DESCRIPTION>[_<SIZE>][_STATE]
```

```
// example
bg_login_background
ic_login_button_small_pressed
```

The `WHAT` denodes the type of the drawable. It can be one of the following:

* ic (Icons)
* bg (Backgrounds)
* shape (Shapes)

The `WHERE` describes in which part of the app the drawable is used. If the drawable is used in more than one screen use `all`.
The `DESCRIPTION` must tell the reader what the actual purpose of the drawable is.
The `SIZE` part is optional and describes the size of the drawable. This can either be a measurable (e.g. 24dp) or a generic term (e.g. small).
`STATE` indicates optionally the state of the drawable. It can be one of the following:

* normal (can be omitted)
* disabled
* selected
* pressed

**[back to top](#table-of-contents)**

## Logging

### Logger

NSLog logs an error message to the Apple System Log facility and therefore must not be used for debug logging.
Besides NSLog - which must not be used in production code - Apple provides a library for logging. This library and the appropiate log level must be used instead of the commonly used NSLog.

```
// example
#import <os/log.h>

@implementation Calculator

- (BOOL)doSomething:(NSString *)tag
{
    // ...
    os_log_debug(OS_LOG_DEFAULT, "%s(%i): Doing something: { tag: %@ }", __FILE__, __LINE__, tag);

    // ...
    os_log_error(OS_LOG_DEFAULT, "%s(%i): Calculation failed: { tag: %@, checksum: %@ }", __FILE__, __LINE__, tag, checksum);

    // ...
}

@end
```

### Loglevels

Apple provides a various amount of log levels each with its own purpose.
Here is a list of log levels and their meaning:

| Level | Function | Meaning |
|---|---|---|
| OS_LOG_TYPE_DEFAULT | os_log | Default-level messages are initially stored in memory buffers. Without a configuration change, they are compressed and moved to the data store as memory buffers fill. They remain there until a storage quota is exceeded, at which point, the oldest messages are purged. Use this level to capture information about things that might result a failure. |
| OS_LOG_TYPE_INFO | os_log_info | Info-level messages are initially stored in memory buffers. Without a configuration change, they are not moved to the data store and are purged as memory buffers fill. They are, however, captured in the data store when faults and, optionally, errors occur. When info-level messages are added to the data store, they remain there until a storage quota is exceeded, at which point, the oldest messages are purged. Use this level to capture information that may be helpful, but isn’t essential, for troubleshooting errors. |
| OS_LOG_TYPE_DEBUG | os_log_debug | Debug-level messages are only captured in memory when debug logging is enabled through a configuration change. They’re purged in accordance with the configuration’s persistence setting. Messages logged at this level contain information that may be useful during development or while troubleshooting a specific problem. Debug logging is intended for use in a development environment and not in shipping software. |
| OS_LOG_TYPE_ERROR | os_log_error | Error-level messages are always saved in the data store. They remain there until a storage quota is exceeded, at which point, the oldest messages are purged. Error-level messages are intended for reporting process-level errors. If an activity object exists, logging at this level captures information for the entire process chain. |
| OS_LOG_TYPE_FAULT | os_log_fault | Fault-level messages are always saved in the data store. They remain there until a storage quota is exceeded, at which point, the oldest messages are purged. Fault-level messages are intended for capturing system-level or multi-process errors only. If an activity object exists, logging at this level captures information for the entire process chain. |

### Logging Messages

Write meaningful log messages. This sounds easy but is in fact really hard. Keep in mind, that you sometimes log for the event of an error, which in reality occurs only rarely. 
But if it does, you depend on a clear log message along with an expressive payload. A good log message must consist of the following items:

* The filename and linenumber of the log statement (for Objective-C this are: `__FILE__` and `__LINE__` and for Swift these are: `#file` and `#line`)
* A meaningful log message
* An expressive payload, usually a JSON (That's why you must always add a description Method - see [here](#classes)

### Logging Message Language

Write your log messages in english. English is a well known language both in terms of writing and reading. Furthermore does it not contain any special characters, which means that it can be logged with ASCII. This is especially important when performing log rotation, since you do not know where your logs are stored.

### Logging Payload

Always log with payload (i.e. context).
The log message often is not sufficient when tracing bugs. You almost always need additional information. So just log them along with the message and you keep yourself from deploying a new version of the app just to improve log messages.

Make sure that the payload is well formatted and complete. The format helps you to filter and/or search for certain events.

```
// good
Transaction failed: { id: 63287, checksum: null }

// bad
Transaction failed!

// bad. Payload included but hard to read/parse
Transaction '63287' failed: Checksum 'null' is invalid!
```

**[back to top](#table-of-contents)**

## Roadmap 

This section describes items, which will be added to this style guide in the future.
These items are categorized in two sections namely `next` for items added in the next release and `future` for items added in future releases without a fixed date.

### Next

* Swift conventions
* Conventions for Logging
* Describe Twine support for cross platform string sharing
* Useful links

### Future 

* Linter for continuous integration based on this style guide

**[back to top](#table-of-contents)**

## Resources

### Documentation

* [http://www.stack.nl/~dimitri/doxygen/manual/docblocks.html](http://www.stack.nl/~dimitri/doxygen/manual/docblocks.html)

### Logging

* [http://doing-it-wrong.mikeweller.com/2012/07/youre-doing-it-wrong-1-nslogdebug-ios.html](http://doing-it-wrong.mikeweller.com/2012/07/youre-doing-it-wrong-1-nslogdebug-ios.html)
* [https://developer.apple.com/documentation/os/logging](https://developer.apple.com/documentation/os/logging)

**[back to top](#table-of-contents)**

### Related Work

This Style Guide is inspired by the following Style Guides:

* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
* [Google Objective-C Style Guide](http://google.github.io/styleguide/objcguide.html)
* [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)
* [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)

## License

(The MIT License)

Copyright (c) 2017 appcom interactive GmbH

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
