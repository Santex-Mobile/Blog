_I know that this really is a basic subject for most developers out there. But, two weeks ago I was asked on the best way for performing a specific task with collections and I really strove before I could give an answer. That's the main reason I did this post, to refresh myself and hopefully help someone too._

Whenever we face the task to store or manage data and groups of objects we <Collections - storing and managing groups of objects.> <recorrer> to collections. In Objective-C we have the well known: **NSArray**, **NSDictionary** and **NSSet**. But we should always keep in mind that, even though we can perform a task with more than one specific one, there are really pros and cons that we should consider before writting code. This could help us gain performance that will make a difference when handling massive-load operations.

These are some of the common tasks for using a collection:

- Enumerating objects
- Find object in collection
- Access object
- Adding object (only on mutables)
- Removing object (only on mutables)


**Collections - Cheat Sheet**

<table style="width:100%">
  <tr>
    <th style="width:30%">NSArray / NSMutableArray</th>
    <th style="width:30%">NSDictionary / NSMutableDictionary</th> 
    <th style="width:30%">NSSet / NSMutableSet</th>
  </tr>
  <tr>
    <td>Ordered elements, easy enumeration, access by index. Slow for checking membership.</td>
    <td>Unordered elements, key-associated elements, elements with meaningful keys. </td> 
    <td>Unordered elements, fast insertion and deletion, fast checking for membership. Used when collection will change incrementally or will be very large. </td>
  </tr>
  <tr>

    <td></td>
    <td>Store values inside a hash table for rapid access to values based on keys.</td> 
    <td>Automatically allocates memory as needed. Objects inside must respond to the NSObject protocol ( methods hash: and IsEqual: ). </td>
  </tr>
</table>

.

There are also some handy classes we can use like **NSCountedSet** subclass of NSMutableSet but for non-distinct objects. **NSIndexSet** that represent a subset of an array storing only the indexes. **NSIndexPath** for managing nested arrays.

But, How is that knowing this information about the Foundation and thinking a couple of times before coding twice help us on our daily work?

**Practicing**

Let's say we have this simple array.

```objectivec

    NSArray *myArray = @[@"Afghanistan",
                         @"Albania",
                         @"Algeria",
                         @"Andorra",
                         @"Angola",
                         @"Antigua and Barbuda",
                         @"Argentina",
                         @"Armenia",
                         @"Australia",
                         @"Austria",
                         @"Azerbaijan",
                         @"Bahamas",
                         @"Bahrain",
                         @"Bangladesh",
                         @"Barbados",
                         @"Belarus",
                         @"Belgium",
                         @"Belize",
                         @"Benin",
                         @"Bhutan",
                         @"Bolivia",
                         @"Bosnia and Herzegovina",
                         @"Botswana",
                         @"Brazil",
                         @"Brunei",
                         @"Bulgaria",
                         @"Burkina Faso",
                         @"Burundi"];
    NSLog(@"Simple NSArray");
    NSLog(@"%@",myArray);

```

And that we are required to find the **repeated items**. One _really common_ approach will be something like the following: ( Spoiler alert : It is using multiple _for loops_. )

**A**

Loop over our array and foreach object loop over the array again (!) this just to verify if there are any other same object.

```objectivec
    //  A: Counting repetitive elements with multiple for-loop
    
    NSMutableArray *elements = [[NSMutableArray alloc] initWithArray:myArray];
    NSMutableSet *myUniqueElementsSet = [NSMutableSet new];
    
    for (int i = 0; i < elements.count; i++) {
        NSString *string = [elements objectAtIndex:i];
        for (int j = i+1; j < elements.count; j++) {
            if ([string isEqualToString:[elements objectAtIndex:j]]) {
                [myUniqueElementsSet addObject:string];
            }
        }
    }
  
    NSLog(@"%@", myUniqueElementsSet);
```

Using method A over 10,000 times we got an average execution time of : 

```objectivec
Average [A]: 53825 ns 
```


**B**

Now, let's do the same task using NSCountedSet.
```objectivec
    //  B: Using NSCountedSet
    
    NSCountedSet *myCountedSet = [[NSCountedSet alloc] initWithArray:myArray];
    NSMutableSet *myUniqueElementsSet = [NSMutableSet new];

    for (id name in myCountedSet) {
        if ([myCountedSet countForObject:name] > 1) {
            [myUniqueElementsSet addObject:name];
        }
    }

    NSLog(@"%@",myUniqueElementsSet);
```

Let's check the execution time for our second implementation. 

```objectivec
Average [B]: 3487 ns
```


As we can see, our B implementation has a better execution time by almost **15.5x times**. And this huge difference was obtained using a simple array of only 47 items.

Now we can see how thinking a couple of times before diving into coding could help us improve the efficiency and execution time of our code.

---

- The method used for measuring execution time is described in  http://nshipster.com/benchmarking/



