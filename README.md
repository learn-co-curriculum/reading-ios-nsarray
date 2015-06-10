# `NSArray` & `NSMutableArray`

## Objectives

1. Understand the classification of collection types.
2. Learn how to create an `NSArray` with literal syntax (`@[]`).
3. Recognize method initializers and learn the role of `nil`.
4. Learn how to query an array with methods and the literal syntax.

## Introduction To Collection Types

A collection types are objects which hold a group of other objects called "a collection". Collections can hold all of a single type (such as string objects) or contain a group of various types (called mixed types), and may or may not track the order of the objects for which they're responsible. 

Since collections are objects themselves, a collection can contain other collections—this is how a matrix is created, by nesting a series of arrays within a parent array. 

**Reminder:** *Primitives are not objects, so they can't be held in a collection. To place a primitive's value in a collection, it must be converted into an* `NSNumber` *object.*

In Objective-C, collections are unaware of the types of the objects that they contain. So, when managing the contents of a collection, it is largely up to the programmer (that's you!) to keep track of what kinds of object are stored and retrieved from the collection.

The common collection base types that you'll encounter in Objective-C are:

```objc
NSArray
NSDictionary
NSSet
```
This reading will cover the basics of the `NSArray` class.

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

The literal syntax for `NSArray` was actually only introduced in iOS 6. Since it's relatively new, it's important to know what the long-form syntax is that literal is calling on your behalf. This way you can recognize array initializers in examples or files that are than older 2012.

```objc
NSArray *instructors = [[NSArray alloc]initWithObjects:@"Joe", @"Tim", @"Jim", @"Tom", @"Mark", nil];
```

```objc
NSArray *instructors = [NSArray alloc];
[instructors initWithObjects:@"Joe", @"Tim", @"Jim", @"Tom", @"Mark", nil];
```

Notice the inclusion of `nil` at the end of the object list; `nil` is a notation that means "no-object" or "nothing". The minding-bending irony of `nil` is that it is actually an object itself, but one that means "not-an-object". It's kind of like a sign that reads, "Post No Signs". Hilarious.

It's significant here because `nil` is how a collection understands its end. The final object in every collection is always `nil`. Since `nil` is the tag for the end of a collection, if you were to successfully put `nil` into a collection a second time, your application would very likely crash at run time because your collection would think it's shorter than it's supposed to be. For this reason, there's a lot of effort in writing Objective-C that gets put into 


### Creating Empty Arrays





====Mark====

There are a number of ways to create a new `NSArray`, some of which instantiate the array with values; others which allow you to specify the values afterward.

######Example
```objc
NSArray *firstArray = [NSArray new];  // This will be an empty array

NSArray *anotherFirstArray = [NSArray array];  // Also empty

NSArray *secondArray = [NSArray alloc] init];  // Also empty

NSArray *thirdArray = @[ @1, @2, @3 ];

NSArray *fourthArray = [NSArray arrayWithArray:thirdArray];  // Creates a copy of another array

NSArray *fifthArray = [NSArray arrayWithObjects: @1, @"Joe", @5, @"Flatiron", nil]; //consider this obsolete or antiquated; the literal syntax used for thirdArray is preferable
```


##Querying `NSArray`

####`containsObject:`

This method will return true if the object parameter is found within the array.

######Example
```objc

NSArray *numbers = @[ @1, @2, @3, @4, @5 ];

BOOL contained = [numbers containsObject:@3]; //contained will be true

```

####`firstObject` / `lastObject`

This method returns the first (or last) object in an array.

######Example
```objc

NSArray *numbers = @[ @1, @2, @3, @4, @5 ];

NSNumber *firstNumber = [numbers firstObject];

NSNumber *lastNumber = [numbers lastObject];

```


###`objectAtIndex`:

This method will return the object at the index specified.

######Example
```objc

NSArray *numbers = @[@1,@2,@3,@4,@5];

NSNumber *numberAtIndexTwo = [numbers objectAtIndex:2];

```

###Literal syntax for `objectAtIndex`

It is also possible to return an object at a specified index using the following shorthand, or literal syntax.

######Example
```objc

NSArray *numbers = @[ @1, @2, @3, @4, @5 ];

NSNumber *numberAtIndexTwo = numbers[2];

```



##Finding objects in `NSArray` objects

####`indexOfObject`:

This method will return the index of a particular object. Remember that array indexes start at 0.

######Example

```objc
NSArray *numbers = @[ @1, @2, @3, @4, @5 ];

NSNumber *theNumberThree = [numbers objectAtIndex:2];
```

##`NSArray` enumeration

###`makeObjectsPerformSelector:`

This method runs the same method for every element in an array (of `SpaceShip` objects in this case.)

######Example
```objc
[spaceShips makeObjectsPerformSelector:@selector(attackEnemy)];

```

###`enumerateObjectsUsingBlock:`

This method lets you perform an operation on the objects inside of an `NSArray`. It comes with a `BOOL` *pointer* which is unusual. The `BOOL` pointer allows you to stop the enumeration of objects if a certain condition is met.

######Example

```objc
NSArray *testArray = @[ @1, @2, @3, @4, @5 ];
    
NSArray *resultsArray = [testArray mapWithOperation:^id(id object) {
        return @([(NSNumber *)object integerValue] + 1);
}];
    
[testArray enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
        
    NSInteger intObj = [(NSNumber *)obj integerValue];
    NSLog(@"%@", @(intObj + 1));
        
    if (intObj == 4) {
        *stop = YES;
    }
}];
```

In the above example, we have chosen to stop the enumeration prematurely by using the `stop` pointer.

###Variations on a theme
There are many other ways to enumerate an `NSArray` with conditions. Take a look at the documentation for more.

##Array sorting
Check out our tutorial on [`NSSortDescriptor`](https://learn.co/lessons/617) for details on how to sort an array.


##Comparing arrays

####`isEqualToArray:`
This method will ensure that every object in one array is equal to every object in another array (ensuring each object conforms to the `isEqual:` method). Keep in mind that `NSArray` objects are only equal if the order of the objects in the arrays are the same, given the basic rules we have learned about them.

######Example

```objc

NSArray *testArrayA = @[ @1, @2, @3, @4 ];
NSArray *testArrayB = @[ @1, @2, @3, @4 ];

if ([testArrayA isEqualToArray:testArrayB]) {
	NSLog(@"The two arrays are equal!");
}

```

#`NSMutableArray`

If you are looking to update an array after it has been created, then you will want an instance of `NSMutableArray` instead of `NSArray`. All of the `NSArray` methods described above are still relevant, and now you get the added benefit of mutability.

But keep in mind that it is better practice to only reveal immutable objects to the users of your classes. In other words, properties and return types should generally be immutable.

###Updating an object at index

###`addObject:`
This method adds an object to the end of an `NSArray`.

Below we add `@3` to the end of our array.

######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[ @1, @2, @3, @4, @5 ]];
        
[mutableTestArray addObject:@3];
```

###`removeObject:`
This method will remove a specified object to an `NSArray`.


Below we remove `@4` from our array.
######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[ @1, @2, @3, @4, @5 ]];
        
[mutableTestArray removeObject:@4];
```


###`removeObjectAtIndex:`
This method will remove a single object at a specified index.

Below we remove `@3` from our array.

######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[ @1, @2, @3, @4, @5 ]];
        
[mutableTestArray removeObjectAtIndex:2];
```


###`removeObjectsInArray:`
This method will remove a collection of objects from an `NSArray`.

Below we remove `@4` and `@2` from our array.
######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[ @1, @2, @3, @4, @5 ]];
        
[mutableTestArray removeObjectsInArray:@[ @4, @2 ]];
```


###`replaceObjectAtIndex:withObject:`
This method replaces an object at a specified index with another object.

Below we replace `@1` with `@6`.
######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[ @1, @2, @3, @4, @5]];
        
[mutableTestArray replaceObjectAtIndex:0 withObject:@6];
```


###`exchangeObjectAtIndex:withObjectAtIndex:`
This method does exactly what it sounds like it does - switches up the indices of two objects.

Below we switch our array from `@[ @1,@2,@3,@4,@5 ]` to `@[ @2,@1,@3,@4,@5 ]`.

######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[ @1,@2,@3,@4,@5 ]];
        
[mutableTestArray exchangeObjectAtIndex:0 withObjectAtIndex:1];
```

###Other options

There are many more convenience methods on `NSMutableArray` that you may find interesting. Check them out in the [documentation](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSMutableArray_Class/)

## Sorting / filtering
Check out our reading on [Sorting Basics](https://learn.co/lessons/617) and on [Filtering Basics](https://learn.co/lessons/7227) to learn more on sorting and filtering collections (`NSArray` and `NSDictionary`).


###Common errors

####Index beyond bounds

If you attempt to access a value outside of the range of the array, you will get the following error.
```
2014-10-30 09:32:49.386 arrayBlocks[13121:622056] *** Terminating app due to uncaught exception 'NSRangeException', reason: '*** -[__NSArrayI objectAtIndex:]: index 5 beyond bounds [0 .. 4]'
```

Here is some example code that will cause such an error.

######Example
```
NSArray *testArray = @[@1,@2,@3,@4,@5];

for (NSInteger i = 0; i <= [testArray count]; i++)
    {
        NSLog(@"%@", testArray[i]);
    }
```

##Conclusion

There are many more cool things you can do with `NSArray` and `NSMutableArray`. Go read the [documentation](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/) (and [here](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSMutableArray_Class/) for `NSMutableArray` to learn more!
