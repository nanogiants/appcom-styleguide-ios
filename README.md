# Appcom iOS Style Guide

This document describes the style guide applied to iOS projects for appcom interactive GmbH. It describes rules how to 
organize your project, dependencies and files, so that some best practises are held.
It is based on the [Appcom General Code Style Guide](http://appcom-bitbucket/projects/APCON/repos/appcom-conventions-general) 
which should be known to understand all parts of this guide.

So use it in your favor if you want to and/or override the style guide in any way you want.

## Tables of Contents

* [Xcode](#xcode)
* [Artifacts](#artifacts)
* [Dependencies](#dependencies)
* [Comments / Doxygen](#comments)
* [Objective-C](#objc-strings)
  * [Strings](#objc-strings)
  * [Classes](#objc-classes)
  * [Naming Conventions](#objc-naming-conventions)
* [Swift](#swift-version)
* [Asset Naming Conventions](#asset-naming-conventions--images)
* [Logging](#logging--logger)
* [Roadmap](#roadmap)
* [License](#license)

## Xcode

### Xcode version

The used Xcode version should always be the latest available stable version. You should not use
a beta version since this can break the continuous integration system.

### Xcode class prefix

In the Project the Organization should be set as `appcom interactive GmbH` and the 
Class-Prefix should be `AC`.

### Xcode structure

Project structure is important since it helps when navigating through the code and has a huge
impact of the overall maintainability of the project. Each group that contains files should refer to a corresponding
folder, so that the files on the harddisk are organized as well. Since iOS Projects should follow the MVC-Pattern, the
folder structure should represent this and keep this Pattern straight forward. The following table will list all folders
that should be present in the project:

| Folder / Group | Description |
|---|---|
| Models | Contains all classes that correspond to the Model in the MVC-Pattern. |
| Views | Contains all classes that correspond to the View in the MVC-Pattern. |
| Controllers | Contains all classes that correspond to the Controller in the MVC-Pattern. |
| Categories | Contains all classes that extend functionalities of well known classes. |
| Storyboards | Contains all storyboards and xib files. |
| Resources | Contains data that is delivered with the app like localization, fonts and databases. |


### Xcode targets

A Xcode-Project contains at least one target. Depending on the project there can be multiple
shared targets. In many cases there are 3 targets that represent the following environments: develop, staging and 
production. If there are changes applied to one of the targets, the other targets should be considered for these changes
too. If for example classes are added, these classes are most likely needed by the other targets too. At the end, if the
implementation work is done, all targets have to at least compile and run. 

**[back to top](#table-of-contents)**

## Artifacts

### Artifact name

The artifact should be named after the following scheme:

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

### Library version

Always use the latest stable version of a library if possible.
Make sure to migrate also major releases if possible.

### Library provision

Libraries should be included using git submodules if compiled within the
project itself or using an artefact repository (like sonatype nexus) if precompiled. Provide a script for installing 
these if needed. The name of the script should be install-deps.sh and placed at the source root.
Don't use any dependency managers like Cocoapods.

### Folder

Dependencies should be found and structured at a well known place. Always place them 
under the `deps` folder at souce root. Precompiled libraries should be placed under the subfolder `lib` and the 
corresponding header files should be under the subfolder `include`. In the Xcode-Project the `Header Search Path` should 
contain the entry `$(SRCROOT)/deps/include` and the `Library Search Path` should contain `$(SRCROOT)/deps/lib`.

**[back to top](#table-of-contents)**

## Comments / Doxygen

### Documentation

Every class and it's methods should contain a description of what it is for and how it
does it's task. To utilize Xcodes doxygen support and to be able to generate a documentation, these descriptions should
use the doxygen syntax. To be uniformly the `!` is used to mark a doxygen comment and `@` to mark commands like `@brief`
and `@param`.

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

Use `/** ... */` for block comments.

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

Use `//` for single line comments.
Place single line comments on a newline above the subject of the comment.
Put an empty line before the comment unless it's on the first line of a block.

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

**[back to top](#table-of-contents)**

## Objective-C

### Objective-C Strings

<a name="strings--format"></a><a name="5.1"></a>
- [5.1](#strings--format)

**[back to top](#table-of-contents)**

### Objective-C Classes

<a name="classes-constructors--constructor-variables"></a><a name="5.2"></a>
- [5.2](#classes-constructors--constructor-variables)

#### Objective-C Class Description

Add always a description method for entities and models. Why: You almost always
have the need to log the current state of a pojo. And that is when you release you get something like this 
`<ACMyClass: 0x170010840>`. So keep yourself from this and create a `- (NSString *)description`-Method.

**[back to top](#table-of-contents)**

### Objective-C Naming Conventions

<a name="naming--descriptive"></a><a name="5.3"></a>
- [5.3](#naming--descriptive)

**[back to top](#table-of-contents)**

## Swift

<a name="swift-version"></a><a name="6.1"></a>
- [6.1](#swift-version) TODO

**[back to top](#table-of-contents)**

## Asset Naming Conventions

### Image Naming

Images should be named after the following pattern:

```
<WHAT>_<WHERE>_<DESCRIPTION>[_<SIZE>][_STATE]
```

```
// example
bg_login_background
ic_login_button_small_pressed
```

The `WHAT` denodes the type of the drawable. It can be one of the
following:

* ic (Icons)
* bg (Backgrounds)
* shape (Shapes)

The `WHERE` describes in which part of the app the drawable is used.
If the drawable is used in more than one screen use `all`. The
`DESCRIPTION` should tell the reader what the actual purpose of the
drawable is. The `SIZE` part is optional and describes the size of the
drawable. This can either be a measurable (e.g. 24dp) or a generic
term (e.g. small). `STATE` indicates optionally the state of the
drawable. It can be one of the following:

* normal (can be omitted)
* disabled
* selected
* pressed

**[back to top](#table-of-contents)**

## Logging

### Logger

NSLog logs an error message to the Apple System Log facility and therefore shouldn't be used for debug logging.
Besides NSLog - which shouldn't be used in production code - Apple provides a library for logging. This library and the appropiate log level should be used instead of the commonly used NSLog.

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

Write meaningful log messages. This sounds easy but is in fact really hard. Keep in mind,
that you sometimes log for the event of an error, which in reality occurs only rarely. 
But if it does, you depend on a clear log message along with an expressive payload. A good log message should consist of
the following items:

* The filename and linenumber of the log statement (for Objective-C this are: __FILE__ and __LINE__ and for Swift these are: #file and #line)
* A meaningful log message
* An expressive payload, usually a JSON (That's why you should always add a description Method - see [here](#classes-constructors--tostring)

### Logging Message Language

Write your log messages in english. English is a well known language both in terms
of writing and reading. Furthermore does it not contain any special characters, which means the it can be logged with
ASCII. This is especially important when performing log rotation, since you do not know where your logs are stored.

### Logging Payload

Always log with payload (i.e. context).
The log message often is not sufficient when tracing bugs. You almost always need additional information. So just log
them along with the message and you keep yourself from deploying a new version of the app just to improve log messages.

Make sure that the payload is well formatted and complete. The format helps you to filter and/or search for certain 
events.

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
These items are categorized in two sections namely `next` for items added in the next release and `future` for items 
added in future releases without a fixed date.

### Next 

* <strike>Conventions for Logging</strike>
* Usage of ORMs in favor of native SQLite Classes
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

## License

(The MIT License)

Copyright (c) 2017 appcom interactive GmbH

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
