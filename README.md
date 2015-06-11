# `NSArray` & `NSMutableArray`

## Objectives

1. Understand the classification of collection types.
2. Learn how to create an `NSArray` with literal syntax (`@[]`).
3. Recognize method initializers and learn the role of `nil`.
4. Learn how to create an `NSMutableArray` and how its type differs from `NSArray`.
5. Learn how to query an array with methods and the array "sugar".
6. Learn how to add and remove objects to and from an `NSMutableArray` with its own methods.

## Introduction To Collection Types

Collection types are objects which hold a group of other objects called "a collection". Collections can hold all of a single type (such as string objects) or contain a group of various types (called mixed types), and may or may not track the order of the objects for which they're responsible. 

Since collections are objects themselves, a collection can contain other collections—this is how a matrix is created, by nesting a series of arrays within a parent array. 

**Reminder:** *Primitives are not objects, so they can't be held in a collection. To place a primitive's value in a collection, it must be converted into an* `NSNumber` *object.*

In Objective-C, collections are unaware of the types of the objects that they contain. So, when managing the contents of a collection, it is largely up to the programmer (that's you!) to keep track of which kind or kinds of objects are stored and retrieved from the collection.

The common collection base types that you'll encounter in Objective-C are:

```objc
NSArray
NSDictionary
NSSet
```
This reading will cover the basics of the `NSArray` class and its mutable type `NSMutableArray`.

## Introduction To `NSArray`

The workhorse of collection types is the array. In Objective-C, arrays take the form of the `NSArray`class. 

**Note:** *The* `array` *class is inherited from C and called 'C-array'. They are not objects and will not behave how you expect, so avoid using them.*

An `NSArray` will manage an ***ordered collection*** of objects. This distinction of managing an **ordered** collection is what makes `NSArray` the go-to class that it is. What this means is that whatever sequence the objects of an `NSArray` are defined in, that same sequence will be preserved for the life of the `NSArray`, or until it is overwritten entirely.

**Advanced:** *The mutable array type* `NSMutableArray` *is capable of altering itself without being directly overwritten. It does, however, still maintain the order of the objects in its managed collection.*

## Creating An `NSArray`

#### The Literal Syntax

You've seen literal syntax used for other object types in previous examples. Being the commonly used type that it is, `NSArray` has a literal syntax. It is `@[]`, with the contents of the array to be created listed inside the square brackets (`[``]`).

```objc
NSArray *instructors = @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ];

NSLog(@"%@", instructors);
```
This will print:

```
(
    Joe,
    Tim,
    Jim,
    Tom,
    Mark
)
```
The description of each object in the array's collection is printed on its own line. You'll notice too that the printout has the contents in the exact same order as the collection we initialized in the code.

#### Initializer Methods

The literal syntax for `NSArray` was actually only introduced in iOS 6. Since it's relatively new, it's important to recognize the long-form syntax that the literal is calling on your behalf. This way you can recognize array initializers in examples or files that are than older 2012.

```objc
NSArray *instructors = [NSArray arrayWithObjects:@"Joe", @"Tim", @"Jim", @"Tom", @"Mark", nil];
```
*This* `+arrayWithObjects:` *class method actually just calls the* `+alloc` `-initWithObjects` *initializer methods together:*

```objc
NSArray *instructors = [[NSArray alloc]initWithObjects:@"Joe", @"Tim", @"Jim", @"Tom", @"Mark", nil];
```
*Before method nesting was supported, they originally had to appear on separate lines:*

```objc
NSArray *instructors = [NSArray alloc];
[instructors initWithObjects:@"Joe", @"Tim", @"Jim", @"Tom", @"Mark", nil];
```
You may come across any of these other examples of creating an array, and you should that you can just use `@[]` as an equivalent.


#### The Role Of `nil`

Notice the inclusion of `nil` at the end of the object list; `nil` is a notation that means "no-object" or "nothing". The minding-bending irony of `nil` is that it is actually an object itself, but one that means "not-an-object". It's kind of like a sign that reads, "Post No Signs". Hilarious. And annoying.

It's significant here because `nil` is how a collection understands its end point. The final object in every collection is always `nil`. Since `nil` is the tag for the end of a collection, if you were to successfully put `nil` into a collection at some earlier point, your application would very likely crash at run time because your collection would think it's shorter than it's supposed to be. For this reason, there's a lot of effort in writing Objective-C that gets put into avoiding defining variables to `nil` whenever possible.


### Creating Empty Arrays

In order to avoid defining an array to `nil`, the array literal can be used to create an empty array.

```objc
NSArray *empty = @[];
```
There are also some antiquated methods on `NSArray` which you might see:

```objc
NSArray *empty = [NSArray new];
```

```objc
NSArray *empty = [NSArray array];
```

```objc
NSArray *empty = [[NSArray alloc]init];
```
**Advanced:** *The array literal is a shorthand for the* `+alloc` `-initWithObjects:` *method pair. The Objective-C language is written to interpret* `@[]` *as calling these methods.*

## The Mutable Type

The mutable type of `NSArray` is `NSMutableArray`. This is apparent from the class names. Being a mutable type means that the object can alter itself without being directly overwritten. For `NSMutableArray`, this means it has additional methods that allow it to add or remove objects from itself.

### Creating An `NSMutableArray`

Unlike `NSArray`, the mutable type `NSMutableArray` doesn't have a literal syntax. This means `NSMutableArray` can only be initialized with methods. Remember the `+alloc` `-initWithObjects:` method on `NSArray`? That *does* also work on `NSMutableArray`:

```objc
NSMutableArray *mInstructors = [[NSMutableArray alloc]initWithObjects:@"Joe", @"Tim", @"Jim", @"Tom", @"Mark", nil];
```
This gives us an equivalent array of instructor names, but as a mutable type.

#### Incorporating The `NSArray` Literal Syntax

What *is* handy about the `NSArray` literal syntax, however, is that we can use it within certain initializers on `NSMutableArray`, specifically `arrayWithArray:`.

```objc
NSArray *instructors = @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ];

NSMutableArray *mInstructors = [NSMutableArray arrayWithArray:instructors];
```
We can also do this in-line with the `NSArray` literal:

```objc
NSMutableArray *mInstructors = [NSMutableArray arrayWithArray: @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ] ]; 
```

#### Creating An Empty `NSMutableArray`

In similar fashion, if we want to create an `NSMutableArray` without any contents, but with the intention of adding contents to it later, we *must* intitialize it to an empty `NSMutableArray`. This is done using the `+alloc` `-init` method pair:

```objc
NSMutableArray *mEmpty = [[NSMutableArray alloc]init];
```
Or the `+new` method:

```objc
NSMutableArray *mEmpty = [NSMutableArray new];
```
Forgetting to initialize a mutable array is a very common oversight in programming Objective-C. *Declaring it is not enough!* All declared-but-undefined variables will get initialized to `nil` by default. Sending a method call to `nil` will cause your application to break. So *always* initialize to an empty array if you have no content to put into it yet!

## Reading Arrays

The purpose of creating arrays is to store information that you need to access later or from somewhere else in your application. 

### Indexing An Array

The most common method for reading the contents of an array—either mutable or static—is the `objectAtIndex:` method. Just like the characters in a string have an index starting with `0` (zero), the objects in an array can be accessed by their index, which also starts with `0` (zero).

**Advanced:** *Indexing works on the letters of a string because strings are actually arrays which specifically hold a sequence of characters.*

In our array of instructor names, for example, "Joe" is the first object at index `0` (zero), while "Jim" is the third object at index `2`.

```objc
NSArray *instructors = @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ];

NSString *joe = [instructors objectAtIndex:0];
NSString *jim = [instructors objectAtIndex:2];

NSLog(@"%@, %@", joe, jim);
```
This will print: `Joe, Jim`.

**Note:** *The* `objectAtIndex:` *method also works on any* `NSMutableArray` *collection.*

### Syntactic Sugar

There's actually a shortand syntax (nicknamed "sugar") which calls the `objectAtIndex:` method and works with *both* `NSArray` and `NSMutableArray` collection types. Like the array literal syntax `@[]`, the array sugar uses the square brackets `[` `]`, but instead of the `@` ("at") symbol, we begin with the name of the array object we wish to index *and then* place the desired index number within the square brackets:

```objc
NSArray *instructors =  @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ];

NSString *joe = instructors[0];
NSString *jim = instructors[2];

NSLog(@"%@, %@", joe, jim);
```
This will also print: `Joe, Jim`.

The array sugar can also be used in-line, such as for the `NSLog()`:

```objc
NSArray *instructors = @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ];

NSLog(@"%@, %@", instructors[0], instructors[2]);
```
This will also print: `Joe, Jim`.

Sugars like this one are useful for quickly reducing verbosity and line-count, and generally improving the readability of your code. Sweetness.

## Utilizing The Mutable Type

The mutable type `NSMutableArray` brings some great additional functionality to ordered collections. According to its title, it is just that—mutable. An `NSMutableArray` can alter itself by adding, removing, or reordering its contents without having to directly overwrite itself.

**Advanced:** *Mutable types actually do overwrite their contents, but they handle it internally giving the appearance of editing themselves. Under the hood, adding or removing an object to or from a mutable array is really just a macro for writing a new array with the speficied object inserted at or removed from the specified index, and then changing its pointer to reference the new array.*

### The `addObject:` Method

This method will add the submitted object to the end of the mutable array. Since it is a `void`-type method, there is no return to capture.

In the example below, our array of instructors was accidentally created with Mark's name spelled incorrectly and with Joe's name in it twice. Oops! Let's start fixing it by adding Mark's name correctly:

```objc
NSMutableArray *mInstructors = [NSMutableArray arrayWithArray:@[ @"Joe", @"Tim", @"Mack", @"Jim", @"Tom", @"Joe" ];

[mInstructors addObject:@"Mark"];

NSLog(@"%@", mInstructors);
```
This will print:

```objc
(
    Joe,
    Tim,
    Mack,
    Jim,
    Tom,
    Joe,
    Mark
)
```
Step one accomplished!

### The `removeObject:` Method

And we can also remove a specified object from the mutable array. Since this method scans the array from the beginning, it will remove *only* the *first matching object* in the array. We can use this method to remove both the offensive spelling of Mark's name and the duplicate of Joe's name:

```objc
[mInstructors removeObject:@"Mack"];
[mInstructors removeObject:@"Joe"];

NSLog(@"%@", mInstructors);
```
This will print:

```objc
(
    Tim,
    Jim,
    Tom,
    Joe,
    Mark
)
```
Great! We're back to five instructor's and Mark is happy that his name is spelled correctly. 

Hm, did you notice, though, that it was the first copy of Joe's name that got removed and not the second?

### The `removeObjectAtIndex:` And `addObject:atIndex:` Methods

There's another pair of methods on `NSMutableArray` which allow us to remove or add an object at a specific spot in the mutable array. Let's use these to move Joe's name back to the front of the list:

```objc
NSString *joe = mInstructors[3];

[mInstructors removeObjectAtIndex:3];
[mInstructors addObject:joe atIndex:0];

NSLog(@"%@", mInstructors);
```
This will print:

```objc
(
    Joe,
    Tim,
    Jim,
    Tom,
    Mark
)
```
Just what we're used to!

**Note:** *The order in which changes are made to an array by index can matter. If we had inserted* `joe` *before removing Joe's name from the array, it would have been Tom's name that would have been at index* `3` *and subsequently removed.*

These are just a few of the additional methods on `NSMutableArray`, but they're the ones you'll interact with the most. When you're ready, refer to the documentation on `NSArray` and `NSMutableArray` to learn more about what these classes can do.