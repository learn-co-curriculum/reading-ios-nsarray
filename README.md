# `NSArray` & `NSMutableArray`

## Objectives

1. Understand the classification of collection types.
2. Create an `NSArray` with literal syntax (`@[]`).
3. Recognize method initializers and learn the role of `nil`.
4. Create an `NSMutableArray` and learn how its type differs from `NSArray`.
5. Query an array with methods and the array "sugar", a.k.a. "subscripting".
6. Add and remove objects to and from an `NSMutableArray` with its own methods.
7. Replace an object in a mutable array by using subscripting.
8. Use the `count` method when iterating over an array with a `for` loop.

## Introduction To Collection Types

### The Usefulness of Collections

Holding information in variables is important, but what happens when we want to hold a lot of individual pieces of information all at once? We *could* create a variable for each piece and this would work okay for small groups of information. But what if, for example, we want to hold all 41,733 United States postal codes at once?

It's in this sort of scenario that holding the information in a collection becomes not only advantageous, but downright necessary. Our examples in this reading, however, will suffice to show a collection of only five objects.

### What Is A Collection?

Collection types are objects which hold a group of other objects called "a collection". A collection can hold all of a single type (such as string objects) or contain a group of various types (called mixed types), and may or may not track the order of the objects for which they're responsible. 

**Top Tip:** *It is highly unusual to put multiple types of objects in the same collection. If you find yourself doing so, think twice.*

Since collections are objects themselves, a collection can contain other collections—this is how a matrix is created, by nesting a series of arrays within a parent array. 

**Reminder:** *Primitives are not objects, so they can't be held in a collection. To place a primitive's value in a collection, it must be converted into an* `NSNumber` *object.*

In Objective-C, collections are unaware of the types of the objects that they contain. So, when managing the contents of a collection, it is largely up to the programmer (that's you!) to keep track of which kind or kinds of objects are stored and retrieved from the collection.

The common collection base types that you'll encounter in Objective-C are:

```objc
NSArray
NSDictionary
NSSet
```
This reading will cover the basics of the array collection type.

## Introduction To `NSArray`

The workhorse of collection types is the array. In Objective-C, arrays take the form of the `NSArray`class. 

An `NSArray` represents an ***ordered collection*** of objects. This distinction of being an **ordered** collection is what makes `NSArray` the go-to class that it is. What this means is that in addition to the objects themselves, the sequence in which they are arranged will also be preserved.

**Advanced:** *As we'll see shortly, the elements of the mutable array type* `NSMutableArray` *can be modified directly. However, it still maintains the order of its objects.*

## Creating An `NSArray`

As mentioned above, if we want to hold the names of our five instructors as string objects, we have to declare and define each of them individually:

```objc
NSString *joe = @"Joe";
NSString *tim = @"Tim";
NSString *jim = @"Jim";
NSString *tom = @"Tom";
NSString *mark = @"Mark";
```
This quickly becomes a lot of work. Instead, we can initialize all of these string objects within the contents of a collection to hold and access them as a group.

#### The Literal Syntax

You've seen the literal syntax used for other object types in previous examples. Being the commonly used type that it is, `NSArray` has a literal syntax, too. It is `@[]`, with the contents of the array to be created listed inside the square brackets (`[``]`). The literal syntax is the most readable and most concise way to create an `NSArray` in your code, and is what you should use in almost all cases of creating an `NSArray`.

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
The description of each object in the array's collection is printed on its own line. You'll notice, too, that the printout has the contents in the exact same order as when we initialized the collection.

#### Initializer Methods

The literal syntax for `NSArray` was actually only introduced in iOS 6. Since it's relatively new, it's important to recognize the long-form syntax that the literal is calling on your behalf. This way you can recognize array initializers in examples you might find on Stack Overflow, or at your future workplace in application files that are older than 2012.

```objc
NSArray *instructors = [NSArray arrayWithObjects:@"Joe", @"Tim", @"Jim", @"Tom", @"Mark", nil];
```
*This* `+arrayWithObjects:` *class method actually just calls the* `+alloc` `-initWithObjects` *initializer methods together:*

```objc
NSArray *instructors = [[NSArray alloc] initWithObjects:@"Joe", @"Tim", @"Jim", @"Tom", @"Mark", nil];
```
You may come across either of these other examples of creating an array, and you should know that you can just use `@[]` as an equivalent.

#### The Role Of `nil`

Notice the inclusion of `nil` at the end of the object list; `nil` is a notation that means "no-object" or "nothing". It's significant here because `nil` acts as the "sentinel value" which tells the `-initWithObjects:` method when to stop looking for new objects. 

**Note:** *One of the advantages of using the array literal syntax is that it doesn't require a sentinel value to mark the end of the objects list.*

#### Creating Empty Arrays

In order to avoid defining an array to `nil`, the array literal alone can be used to create an empty array.

```objc
NSArray *empty = @[];
```
**Advanced:** *The array literal is a shorthand for the* `+alloc` `-initWithObjects:` *method pair. The Objective-C language is written to interpret* `@[]` *as calling these methods.*

## The Mutable Array

So far we've only been discussing static, or immutable, arrays—that is, arrays whose elements are known at the time of creation and do not change. But what if we want to change the contents of an array after we've defined it? 

Enter `NSMutableArray`, the mutable counterpart to `NSArray`. The "mutable" nature means that the object can be altered after its definition. For `NSMutableArray`, this means it has additional methods that allow it to add or remove objects from itself.

### Creating An `NSMutableArray`

Unlike `NSArray`, the mutable type `NSMutableArray` doesn't have a literal syntax. This means `NSMutableArray` can only be initialized with methods. The easiest way is perhaps the `mutableCopy` method that can be called upon an `NSArray`:

```objc
NSArray *instructors = @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ];
NSMutableArray *mInstructors = [instructors mutableCopy];
```
This `mutableCopy` method returns a mutable array with identical contents.

We can also do this on a single line by passing an `NSArray` literal in the `arrayWithArray:` method on `NSMutableArray`:

```objc
NSMutableArray *mInstructors = 
    [NSMutableArray arrayWithArray:@[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ] ]; 
```

#### Creating An Empty `NSMutableArray`

In similar fashion, if we want to create an `NSMutableArray` without any contents, but with the intention of adding contents to it later, we *must* initialize it to an empty `NSMutableArray`. The most universal syntax that you'll see for this is the `+alloc` and `-init` method pair:

```objc
NSMutableArray *mEmpty = [[NSMutableArray alloc] init];
```
We'll discuss these methods in more detail later; for now, know that they're necessary for creating variables. The literal syntaxes that you've learned previously call this pair of methods implicitly. However, `NSMutableArray` does not have it's own literal syntax, so mutable arrays must be created using methods. 

Some other creation methods that you might see used with `NSMutableArray` in examples online are the `new` or `array` methods:

```objc
NSMutableArray *mEmpty = [NSMutableArray new];
```

```objc
NSMutableArray *mEmpty = [NSMutableArray array];
```

These implicitly call the `+alloc` and `-init` methods, they're just written in one method call instead of two.

**Top-tip:** *While the* `new` *and* `array` *methods are entirely valid, we strongly suggest getting into the habit of writing out the* `alloc` *and* `init` *method pair when creating new variables that don't have a literal syntax. It's easy to convert the base* `init` *method to a specific one such as* `initWithObjects:`*, making for more-easily maintainable code.*

#### Always Initialize Mutable Arrays

Forgetting to initialize a mutable array, particularly when declared as a property (we'll explain properties in a later topic), is a very common oversight in programming Objective-C and can be a very difficult bug to diagnose. In this case, the mutable array can be the recipient of method calls but won't actually retain any of the values submitted to it, so attempting to access the contents of the array later will return `nil`.

```objc
NSMutableArray *mInstructors;

[mInstructors addObjectsFromArray:@[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ] ];

NSLog(@"%@", mInstructors);
```
This will print: `(null)`, which is how `nil` is expressed by LLDB in the debug console.

Oops.

## Reading Arrays 

The whole point of storing information in a collection is to access it later. The most common way to access the contents of an array is by index.

#### Syntactic Sugar: Subscripting

There's a useful shorthand syntax (nicknamed "sugar" or "subscripting") which works with both `NSArray` and `NSMutableArray` collections. Subscripting returns the object at the submitted index number of the array being accessed. It is written out as an integer (or integer variable) within square brackets `[]` following the array's name. Just like the characters in a string which begin at index `0` (zero), the objects in an array also begin at index `0` (zero).

In our array of instructor names, for example, "Joe" is the first object at index `0` (zero), while "Jim" is the third object at index `2`.

```objc
NSArray *instructors =  @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ];

NSString *joe = instructors[0];
NSString *jim = instructors[2];

NSLog(@"%@, %@", joe, jim);
```
This will print: `Joe, Jim`.

Subscripting can also be used in-line, such as for the `NSLog()`:

```objc
NSArray *instructors = @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ];

NSLog(@"%@, %@", instructors[0], instructors[2]);
```
This will also print: `Joe, Jim`.

Sugars like this one are useful for quickly reducing verbosity and line-count, and generally improving the readability of your code. Sweetness.

#### The `objectAtIndex:` Method

Using the subscript syntax in this manner is actually just calling the `objectAtIndex:` method for the submitted index number.

```objc
NSArray *instructors = @[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mark" ];

NSString *joe = [instructors objectAtIndex:0];
NSString *jim = [instructors objectAtIndex:2];

NSLog(@"%@, %@", joe, jim);
```
This will print: `Joe, Jim`.

**Note:** *The* `objectAtIndex:` *method also works on any* `NSMutableArray` *collection.*

## Methods Of Mutability

The mutable type `NSMutableArray` brings some great additional functionality to ordered collections. According to its title, it is just that—mutable. Unlike its static counterpart,  `NSMutableArray` can alter itself by adding, removing, or reordering its contents after being defined.

#### The `addObject:` Method

This method will add the submitted object to the end of the mutable array. Since it is a `void`-type method, there is no return to capture.

In the example below, our array of instructors was accidentally created with Mark's name spelled incorrectly and with Joe's name included twice. Oops! Let's start fixing it by adding Mark's name correctly:

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

#### The `removeObject:` Method

And we can also remove a specified object from the mutable array. We can use this method to remove both the offensive spelling of Mark's name and the duplicate of Joe's name:

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
    Mark
)
```
Uh oh! Joe's name is gone from the array completely. It looks like the `removeObject:` method got rid of both occurrences of Joe's name in the array. Calling the `removeObject:` method will remove every object from the array that matches the submitted argument. 

#### The `removeObjectAtIndex:` And `insertObject:atIndex:` Methods

There's another pair of methods on `NSMutableArray` which allow us to remove or add an object at a specific spot in the mutable array. We could have used the `removeObjectAtIndex:` method above to remove *just* the second occurrence of Joe's name. Let's use the `insertObject:atIndex:` method to put Joe's name back at the front of the list:

```objc
NSString *joe = @"Joe";

[mInstructors insertObject:joe atIndex:0];

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

#### Subscript Setting And `replaceObjectAtIndex:withObject:`

An additional ability of `NSMutableArray`s allow them to utilize the array subscript (`[]`) to replace an object at the submitted index. This looks just like the reverse of using the subscript syntax to retrieve the contents of the array:

```objc
NSMutableArray *mInstructors = 
     [NSMutableArray arrayWithArray:@[ @"Joe", @"Tim", @"Jim", @"Tom", @"Mike" ] ];

mInstructors[4] = @"Mark";

NSLog(@"%@", mInstructors);
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
It is equivalent to calling `NSMutableArray`'s `replaceObjectAtIndex:withObject:` method and will *only* work with mutable arrays:

```objc
[mInstructors replaceObjectAtIndex:4 withObject:@"Mark"];
```
However, neither the subscript nor this method call can be used to insert a new object into the array—it can only replace an object at an existing index. Your application will crash with an `index beyond bounds` error if you're not careful.

#### The `count` Method

Sometimes you will need to know how many elements are contained in an array. You can get this information by calling the `count` method on the array in question. The most common usage case of this is when limiting the number of iterations that a  `for` loop should run over the array. Let's say we have a loop that `NSLog()`s a greeting to each student whose name is held in an array:

```objc
NSArray *students = @[ @"Joe", @"Mark" ];

for (NSUInteger i = 0; i < 2; i++) {
    NSLog(@"Welcome, %@!", students[i] );
}
```

But what happens when we add a new student `@"Tom"` to the array? 

```
NSArray *students = @[ @"Joe", @"Mark", @"Tom"];
```

We would have to manually change our `for` loop's conditional to look like this:

```objc
for (NSUInteger i = 0; i < 3; i++) {...}
```

How did we know it needed to be `3` this time and not `2`? Well, because we manually counted the number of elements in the array! However, we can get our `for` loop to count the number of elements in the array for us *at run time*. This means that our `for` loop can handle a `students` array of *any* length just by having it call this handy `count` method and using its result to limit the `for` loop's counter. Implementing the `count` method in our `for` loop's conditional statement would like this:

```objc
for (NSUInteger i = 0; i < [students count]; i++) {...}
```

Using the `count` method also protects our loop from iterating *too many* times over an array, which would cause the application to crash with an `index ? beyond bounds [range]` error:

```objc
// bad example

NSArray *students = @[ @"Joe", @"Mark"];
for (NSUInteger i = 0; i < 3; i++) {
    NSLog(@"Welcome, %@!", students[i] );
}
```
This will cause a crash that prints:

```
Welcome, Joe!
Welcome, Mark!
*** Terminating app due to uncaught exception 'NSRangeException', reason: '*** -[__NSArrayI objectAtIndex:]: index 2 beyond bounds [0 .. 1]'
*** First throw call stack:
```

Yikes! In order to avoid this problem, it's a best practice of "defensive programming" to *always* use the `count` method when iterating over an array:

```objc
// good example

NSArray *students = @[ @"Joe", @"Mark"];
for (NSUInteger i = 0; i < [students count]; i++) {
    NSLog(@"Welcome, %@!", students[i] );
}
```
This will print:

```
Welcome, Joe!
Welcome, Mark!
```
No crash this time!

## Conclusion

These are just a few of the additional methods on `NSMutableArray`, but they're the ones you'll interact with the most. When you're ready, refer to the documentation about `NSArray` and `NSMutableArray` to learn more about what arrays can do.

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/reading-ios-nsarray' title='NSArray & NSMutableArray'>NSArray & NSMutableArray</a> on Learn.co and start learning to code for free.</p>

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/reading-ios-nsarray'>NSArray & NSMutableArray</a> on Learn.co and start learning to code for free.</p>
