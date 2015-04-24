#`NSArray`


##What is a collection?

A collection is simply a group of objects. A collection could be all of one type (i.e. a series of `NSString` objects) or of mixed types.



##Basics of `NSArray`

`NSArray` is the most common collection object, and abides by certain basic rules. 

These are:

1) We begin counting objects in an `NSArray` from 0. In other words, the first object in an array is Object 0.
2) `NSArray` objects are ordered and will maintain the order of the objects they contain.
3) `NSArray` objects are immutable; they may not be modified once objects have been added to them (or if created with nil objects.)
4) Given that `NSArray` objects can contain mixed data types, it is generally a good idea to make sure to use introspection when pulling objects out of the collection to ensure you have an object of the type you are seeking. If you haven't yet learned about introspection, check out the reading [here](link to come).



##Creating an `NSArray`

There are a number of ways to create a new `NSArray`, some of which instantiate the array with values; others which allow you to specify the values afterward.

######Example
```objc
NSArray *firstArray = [NSArray new];

NSArray *secondArray = [NSArray alloc] init];

NSArray *thirdArray = @[@1,@2,@3];

NSArray *fourthArray = [NSArray arrayWithArray:thirdArray];

NSArray *fifthArray = [NSArray arrayWithObjects: @1, @"Joe", @5, @"Flatiron", nil]; //consider this obsolete or antiquated, given literal syntax
```

Note: For reasons we will not go into here, we do not suggest using `[NSArray array]` to initialize a new `NSArray`.


##Querying `NSArray`

####`containsObject:`

This method will return true if the object parameter is found within the array.

######Example
```objc

NSArray *numbers = @[@1,@2,@3,@4,@5];

BOOL contained = [numbers containsObject:@3]; //contained will evaluate to true

```

####`firstObject` / `lastObject`

This method returns the first (or last) object in an array.

######Example
```objc

NSArray *numbers = @[@1,@2,@3,@4,@5];

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

NSArray *numbers = @[@1,@2,@3,@4,@5];

NSNumber *numberAtIndexTwo = numbers[2];

```



##Finding objects in `NSArray` objects

####`indexOfObject`:

This method will return the index of a particular object.

######Example

```objc
NSArray *numbers = @[@1,@2,@3,@4,@5];

NSNumber *theNumberThree = [numbers objectAtIndex:2];
```

##`NSArray` enumeration

###`makeObjectsPerformSelector:`

This method runs the same method for every element in an array (of `SpaceShip` objects in this case.)

######Example
```objc
[spaceShips makeObjectsPerformSelector:@selector(attackEnemy:)];

```

###`enumerateObjectsUsingBlock:`

This method lets you perform an operation on the objects inside of an `NSArray`. It comes with a `BOOL` *pointer* which is unusual. The `BOOL` pointer allows you to stop the enumeration of objects if a certain condition is met.

######Example

```objc
NSArray *testArray = @[@1,@2,@3,@4,@5];
    
NSArray *resultsArray = [testArray mapWithOperation:^id(id object) {
        return @([(NSNumber *)object integerValue] + 1);
}];
    
[testArray enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
        
    NSInteger intObj = [(NSNumber *)obj integerValue];
    NSLog(@"%@",@(intObj + 1));
        
    if (intObj == 4) {
        *stop = YES;
    }
}];
```

In the above example, we have chosen to stop the enumeration prematurely by using the `stop` pointer.

###Variations on a theme
There are many other ways to enumerate an `NSArray` with conditions. Take a look at the documentation for more.

##Array sorting
Check out our tutorial on [`NSSortDescriptor`](https://github.com/learn-co-curriculum/reading-ios-sorting-basic) for details on how to sort an array.


##Comparing arrays

####`isEqualToArray:`
This method will ensure that every object in one array is equal to every object in another array (ensuring each object conforms to the `isEqual:` method). Keep in mind that `NSArray` objects are only equal if the order of the objects in the arrays are the same, given the basic rules we have learned about them.

######Example

```objc

NSArray *testArrayA = @[@1,@2,@3,@4];
NSArray *testArrayB = @[@1,@2,@3,@4];

if ([testArrayA isEqualToArray:testArrayB]) {
	NSLog(@"The two arrays are equal!");
}

```

#NSMutableArray

If you are looking to update an array after it has been created, then you will want an instance of `NSMutableArray` instead of `NSArray`. All of the `NSArray` methods described above are still relevant, and now you get the added benefit of mutability.

But keep in mind that it is better practice to only reveal immutable objects to the users of your classes. In other words, properties and return types should generally be immutable.

###Updating an object at index
```
###`addObject:`
This method adds an object to the end of an `NSArray`.

Below we add `@3` to the end of our array.

######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[@1,@2,@3,@4,@5]];
        
[mutableTestArray addObject:@3];
```


###`removeObject:`
This method will remove a specified object to an `NSArray`.


Below we remove `@4` from our array.
######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[@1,@2,@3,@4,@5]];
        
[mutableTestArray removeObject:@4];
```


###`removeObjectAtIndex:`
This method will remove a single object at a specified index.

Below we remove `@3` from our array.

######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[@1,@2,@3,@4,@5]];
        
[mutableTestArray removeObjectAtIndex:2];
```


###`removeObjectsInArray:`
This method will remove a collection of objects from an `NSArray`.

Below we remove `@4` and `@2` from our array.
######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[@1,@2,@3,@4,@5]];
        
[mutableTestArray removeObjectsInArray:@[@4,@2]];
```


###`replaceObjectAtIndex:withObject:`
This method replaces an object at a specified index with another object.

Below we replace `@1` with `@6`.
######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[@1,@2,@3,@4,@5]];
        
[mutableTestArray replaceObjectAtIndex:0 withObject:@6];
```


###`exchangeObjectAtIndex:withObjectAtIndex:`
This method does exactly what it sounds like it does - switches up the indices of two objects.

Below we switch our array from `@[@1,@2,@3,@4,@5]` to `@[@2,@1,@3,@4,@5]`.
######Example
```objc
NSMutableArray *mutableTestArray = [[NSMutableArray alloc] initWithArray:@[@1,@2,@3,@4,@5]];
        
[mutableTestArray exchangeObjectAtIndex:0 withObjectAtIndex:1];
```

###Other options

There are many more convenience methods on `NSMutableArray` that you may find interesting. Check them out in the [documentation](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSMutableArray_Class/)

## Sorting / filtering
Check out our reading on [Sorting Basics](https://github.com/learn-co-curriculum/reading-ios-sorting-basic) and on [Filtering Basics](https://github.com/learn-co-curriculum/reading-ios-filtering-basic) to learn more on sorting and filtering collections (`NSArray` and `NSDictionary`).


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