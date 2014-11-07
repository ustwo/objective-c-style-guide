# ustwo™ Objective-C Style Guide

## Table of contents

* [Spaces Vs. Tabs](#spaces-vs-tabs)
* [Line Length](#line-length)
* [Type Safety](#type-safety)
* [Methods](#methods)
    * [Naming](#naming)
    * [Empty Methods](#empty-methods)
    * [Curly Brackets](#curly-brackets)
    * [Extending existing methods](#extending-existing-methods)
    * [Private Methods](#private-methods)
* [Overriding Methods](#overriding-methods)
* [Method Invocations](#method-invocations)
* [Protocols](#protocols)
* [Blocks](#blocks)
* [Class Names](#class-names)
* [Variable Names](#variable-names)
* [Instance Variables](#instance-variables)
* [Constants](#constants)
* [Enumerations](#enumerations)
* [Notifications](#notifications)
* [Asset Names](#asset-names)
* [Sequence Of Methods](#sequence-of-methods)
* [Imports](#imports)
* [Pragma Marks](#pragma-marks)
* [Comments](#comments)

- - -

## Spaces vs. Tabs

We use spaces for indentation. Do not use tabs in your code. You should set your editor to emit spaces when you hit the tab key. One tab equals 4 spaces.

## Line Length
Try to avoid long lines. Use soft wrap if possible and wherever readability requires it we split the line. Just use common sense.

## Type Safety
Be as type safe as possible. Avoid using id or base classes like NSObject, UIView where possible. Use the lowest common multiple preferring a protocol or an abstract class.

Don’t use:

``` objective-c
+ (id)viewWithData:(id)data
{
    US2Class *classInstance = [[US2Class alloc] initWithData:(US2Data *)data]
    return classInstance;
}
```

Use:

``` objective-c
+ (US2Class *)viewWithData:(US2Data *)data
{
    US2Class *classInstance = [[US2Class alloc] initWithData:data];
    return classInstance;
}
```

## Methods
### Naming
In general, don’t abbreviate names of things. Spell them out, even if they’re long.

See this url for more information:
[http://developer.apple.com/library/ios/#DOCUMENTATION/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html](http://developer.apple.com/library/ios/#DOCUMENTATION/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)

Don’t use:

    destSel:
    setBkgdColor:

Use:

    destinationSelection:
    setBackgroundColor:

One space should be used between the `–` or `+` and the return type, and no spacing in the parameter list except between parameters.

Methods should look like this:

``` objective-c
- (void)doSomethingWithSpecialString:(NSString *)specialString
{
    // ...
}
```

If you have too many parameters to fit on one line, giving each its own line is preferred. If multiple lines are used, align each using the colon before the parameter. You are free to wrap also methods with two parameters.

``` objective-c
- (void)doSomethingWith:(US2Foo *)foo
              rectangle:(CGRect)rectangle
               interval:(NSTimeInterval)interval
{
    // ...
}
```

### Empty Methods

Avoid having empty methods in classes unless they are overriding a method and it needs to be empty.
Don’t use:

``` objective-c
- (void)viewDidLoad
{
    [super viewDidLoad];
}
```

### Curly Brackets

Curly brackets on new line but not for blocks. Blocks should use brackets on first line because Xcode is reformatting in an ugly way.

### Extending existing methods

Add new keywords to the end of an existing method when you create a method that is more specific than the inherited one.

``` objective-c
- (id)initWithFrame:(NSRect)frameRect;
- (id)initWithFrame:(NSRect)frameRect mode:(int)aMode cellClass:(Class)factoryId;
```

## Private Methods

Private methods is named in the same manner as public methods. The methods should be sorted after functionality and organized with pragma marks. The public and private methods related to one functionality are placed together in one group. Their order is public methods first, then private. This also makes the Xcode function list more readable.


#pragma mark - Methods related to function X

``` objective-c
- (void)myPublicMethod
{
    // ...
}
```


#pragma mark-Private

``` objective-c
- (void)myPrivateMethodRelatedToMyPublicMethod
{
    // ...
}
```

When creating public libraries there is a good idea to use prefix on your private methods to prevent unintentional overriding. The prefix should be “US2_”.

## Overriding Methods

Always call the super method unless you have to prevent the super call.

## Method Invocations

Invocations should have all arguments on one line:

``` objective-c
[myObject doFooWith:arg1 name:arg2 error:arg3];
```

or have one argument per line, with colons aligned:

``` objective-c
[myObject doFooWith:arg1
               name:arg2
              error:arg3];
```

Don’t use any of these styles:

``` objective-c
[myObject doFooWith:arg1 name:arg2  // some lines with >1 arg
                  error:arg3];
[myObject doFooWith:arg1
                   name:arg2 error:arg3];
[myObject doFooWith:arg1
              name:arg2  // aligning keywords instead of colons
              error:arg3];
```

## Protocols

There must be a space between the type identifier and the name of the protocol encased in angle brackets.

``` objective-c
@interface US2Class : NSObject <US2Delegate>
{
    @private
    UIView *_privateView;
}

@end
```

Define callback APIs with @protocol, using @optional if not all the methods are required.

## Blocks

Blocks are preferred to the target-selector pattern when creating callbacks, as it makes code easier to read.

There are several appropriate style rules, depending on how long the block is:

* If the block can fit on one line, no wrapping is necessary.
* If it has to wrap, the closing brace should line up with the first character of the line on which the block is declared.
Code within the block should be indented four spaces.
* If the block takes no parameters, there are no spaces between the characters `^{`. If the block takes parameters, there is no space between the `^(` characters, but there is one space between the `) { `characters.

Examples:

The entire block fits on one line.


``` objective-c
[operation setCompletionBlock:^{ [self onOperationDone]; }];
```

The block can be put on a new line, indented four spaces, with the closing brace aligned with the first character of the line on which
 block was declared.

``` objective-c
[operation setCompletionBlock:^{
    [self.delegate newDataAvailable];
}];
```

 Using a block with a C API follows the same alignment and spacing rules as with Objective-C.

``` objective-c
dispatch_async(fileIOQueue_, ^{
    NSString* path = [self sessionFilePath];
    if (path) {
      // ...
    }
});
```


An example where the parameter wraps and the block declaration fits on the same line. Note the spacing of `^(SessionWindow *window) {` compared to `^{` above.


``` objective-c
[[SessionService sharedService] loadWindowWithCompletionBlock:^(SessionWindow *window) {
    if (window)
    {
        [self windowDidLoad:window];
    }
    else
    {
        [self errorLoadingWindow];
    }
}];
```

An example where the parameter wraps and the block declaration does not fit on the same line as the name.

``` objective-c
[[SessionService sharedService] loadWindowWithCompletionBlock:^(SessionWindow *window) {
    if (window)
    {
    [self windowDidLoad:window];
    }
    else
    {
    [self errorLoadingWindow];
    }
}];
```

Large blocks can be declared out-of-line.

``` objective-c
void (^largeBlock)(void) = ^{
    // ...
};
[operationQueue_ addOperationWithBlock:largeBlock];
```


## Class Names

Class names (along with category and protocol names) should start as uppercase and use mixed case to delimit words.

Use prefixes when naming classes, protocols, functions, constants, and typedef structures. Do not use prefixes when naming methods; methods exist in a name space created by the class that defines them.

Also, don’t use prefixes for naming the fields of a structure.
In application-level code, prefixes on class names should generally be avoided. Having every single class with same prefix impairs readability for no benefit. When designing code to be shared across multiple applications, prefixes are acceptable and recommended (e.g. US2SendMessage).

## Variable Names

Variables names start with a lowercase and use mixed case to delimit words. Give as descriptive a name as possible, within reason. Don’t worry about saving horizontal space as it is far more important to make your code immediately understandable by a new reader. Another disadvantage of using abbreviations is that everybody uses different.

A good example is ‘number’ which can be ‘nr’, ‘nbr’, ‘no’ or ‘num’. To be consistent use ‘number’ – everybody knows how to write ‘number’. Make sure the name of the variable concisely describes the attribute stored.

Don’t use:

``` objective-c
int i;
int numErr;
UIView *bgView;
UIViewController *loginVC;
```
Use:


``` objective-c
int index;
int numberOfErrors;
UIView *backgroundView;
UIViewController *loginViewController;
```

## Instance Variables

Do not create public or private instance variables. Developers should concern themselves with an object’s interface, not with the details of how it stores its data.

Create private properties as an alternative to private instance variables, accessing and setting them through their setters and getters. 

This avoids progammer errors such as a property with a `copy` attribute being assigned as:

``` objective-c
_title = string
```

 instead of:

``` objective-c
_title = [string copy]
```

As well as inadvertently avoiding custom setter/getter logic. 

In summary:

Don't use

``` objective-c
_titleLabel.text = @"Hello World"
```

Use:

``` objective-c
self.titleLabel.text = @"Hello World"
```

## Constants

In general, don’t use the #define preprocessor command to create constants. For integer constants, use enumerations, and for floating point constants use the const qualifier. To publicise the constant use extern command.

Don’t use:


``` objective-c
const CGFloat cUS2Constant
const CGFloat US2Constant
const CGFloat US2_CONSTANT
```

Use:

``` objective-c
const CGFloat kUS2Constant
```

## Enumerations

Use enumerations for groups of related constants that have integer values.

``` objective-c
typtypedef enum {
    NSMatrixModeRadio           = 2,
    NSMatrixModeHighlight       = 3,
    NSMatrixModeList            = 4,
    NSMatrixModeTrack           = 5
} NSMatrixMode;
```

You can create unnamed enumerations for things like bit masks, for example:

``` objective-c
enum {
    NSWindowMaskBorderless      = 0,
    NSWindowMaskTitled          = 1 << 0,
    NSWindowMaskClosable        = 1 << 1,
    NSWindowMaskMiniaturizable  = 1 << 2,
    NSWindowMaskResizable       = 1 << 3
};
```

## Notifications

    [Name of associated class] + [Did | Will] + [UniquePartOfName] + Notification

Examples:

``` objective-c
NSApplicationDidBecomeActiveNotification
NSWindowDidMiniaturizeNotification
NSTextViewDidChangeSelectionNotification
NSColorPanelColorDidChangeNotification
```

## Asset Names

Use descriptive file namings. Describe what the asset is used for, whether it is a background, button or image. Delimit the words by underscore. Use only lowercase. For the status use interface builder’s statuses.

    <module>_<type>_<status><retina; 4" or 3.5">.png

* `login_background@2x.png`
* `login_background-568h@2x.png`
* `login_button_register_normal@2x.png`
* `login_button_register_highlighted@2x.png`
* `login_button_register_disabled@2x.png`
* `login_button_register_selected@2x.png`
* `login_icon_company.png`
* `settings_cell_label_background_company.png`

## Sequence Of Methods

Order the methods by their functionality. Also write initializing methods before de-initializing methods.

Use:

``` objective-c
#pragma mark - Init & dealloc

- (void)init
{
    // ...
}

- (void)dealloc
{
    // ...
}

#pragma mark - Methods related to function X


- (void)myPublicMethodX
{
    // ...
}


#pragma mark-Private


- (void)myPrivateMethodRelatedToMyPublicMethodX
{
    // ...
}


#pragma mark - Methods related to function Y


- (void)myPublicMethodY
{
    // ...
}


#pragma mark-Private


- (void)myPrivateMethodRelatedToMyPublicMethodY
{
    // ...
}


#pragma mark - Methods related to function Y


- (void)someDelegateMethod
{
    // ...
}
```

## Imports

* Import framework headers first
* Import header file second after empty line
* Import private category third (if available)
* Sort the imports by type or functionality
* Remove unused imports
* Add two empty lines after imports

Use:

``` objective-c
#import <Foundation/Foundation.h>
#import <MapKit/MapKit.h>
// Empty line
#import "US2Class.h"               // (1) Own header
#import "US2ClassPrivate.h"        // (2) Own private category
#import "DashboardTableViewCell.h" // (3) UI
#import "LoginTableViewCell.h"
#import "RegisterButton.h"
#import "AcceptanceAlertView.h"
#import "LoginModel.h"             // (4) Data
#import "LoginVO.h"
#import "DataProvider.h"
// Empty line
// Empty line
@implementation US2Class
@end
```

## Pragma Marks

Section your code by using pragma marks on one line. Pragma marks are dividing the functionalities in a class or file. For example:

``` objective-c
// Empty line
// Empty line
#pragma mark - Initialization
```

Add two empty lines before a pragma mark to create a more obvious segmentation of code. Add one line after a pragma mark.

## Comments

### File Comments

Start each file with an author and a copyright notice and/or license boilerplate, then followed by a basic description of the contents of the file.

If you make significant changes to a file that someone else originally wrote, add yourself to the author line. This can be very helpful when another contributor has questions about the file and needs to know whom to contact about it.

***

    The MIT License (MIT)

    Copyright (c) 2013 ustwo™

    Permission is hereby granted, free of charge, to any person obtaining a copy of
    this software and associated documentation files (the "Software"), to deal in
    the Software without restriction, including without limitation the rights to
    use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
    the Software, and to permit persons to whom the Software is furnished to do so,
    subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
    FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
    COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
    IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
    CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
