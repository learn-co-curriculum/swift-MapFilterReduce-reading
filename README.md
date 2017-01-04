# Sort, Map and Filter


![](http://i.imgur.com/aaMDoFU.png?1)

> Language is never neutral. -[Paulo Freire](https://en.wikipedia.org/wiki/Paulo_Freire)

## Overview

In this lesson, we'll look at functions that you can use with arrays. 

## Learning Objectives

* Create and use the `sort` function 
* Use the `map` function and explain its return value 
* Create and use the `filter` function 


## Videos

**Map**:

[![Map](http://img.youtube.com/vi/Q3wfJCfxhnw/0.jpg)](https://www.youtube.com/watch?v=Q3wfJCfxhnw "Map")

**Filter and Reduce**:

[![F](http://img.youtube.com/vi/ievEyDNq2WU/0.jpg)](https://www.youtube.com/watch?v=ievEyDNq2WU "F")



## Sort

Who doesn't love Neil Diamond? 

![](http://i.imgur.com/DfLfkKf.png?1)

> Money talks, but it don’t sing and dance and it don’t walk. -Neil

```swift
let greatestHits = [
    "Cracklin' Rosie",
    "Forever in Blue Jeans",
    "Song Sung Blue",
    "Sweet Caroline"
]
```

What if we want to reverse the order of this array? Without using the `reverse()` method available to instances of an `Array`, lets go about this slightly differently to help you understand the topic at hand.

Any instance of an `Array` has functions available to it (thanks to Apple). 

![](http://i.imgur.com/BlLGkMb.png?1)

Here, I'm beginning to type out `sort` on the `greatestHits` constant. I highlighted the one I'm interested in, take a look at the signature of that function we want to call on here. Its return type is [`String`]. The name of the function is `sort` and it has one argument called `isOrderedBefore` which is of type `(String, String) -> Bool`. 

We know how to read that now. We now how to provide this `sort` function with the argument it's looking. It wants a function that has two arguments, both of which must be `String`'s and it must return a `Bool`. 

Lets create that function.

```swift
func backwardsFunction(a: String, _ b: String) -> Bool {
    
    return a > b
    
}
```

```swift
let apple = "Apple"
let zebra = "Zebra"

let result = backwardsFunction(apple, zebra)
// false
```

When you use the `<` or `>` operator with `String`'s. The letter a is considered less than the letter z. So z is greater than a. So z > a is equal to `true`.

The reason our function above returned `false` is because the word Apple is not greater than Zebra because Z is greater than A so it returns `false`.

We've now created a function that has two arguments both of `String` and returns `Bool`. This matches up to what the `sort` function was looking for. 

```swift
let backwardsHits = greatestHits.sort(backwardsFunction)
print(backwardsHits)
// prints "["Sweet Caroline", "Song Sung Blue", "Forever in Blue Jeans", "Cracklin' Rosie"]"
```

We're calling on the `sort` function and providing it with what it wants. It wants a function that has two arguments (both of type `String`) and returns a `Bool`. Thats exaclty what `backwardsFunction` is. So we pass `backwardsFunction` as an argument to the `sort` function. The `sort` function has been implemented by Apple (so we can't see that source code), but you can imagine that its implementation will call our `backwardsFunction` over and over in producing this new array. Well.. we can add print statements, lets do that to see how many times it's getting called by this `sort` function.

```swift
func backwardsFunction(a: String, _ b: String) -> Bool {

    print("backwardsFunction is called with \(a) and \(b)")
    
    return a > b
    
}
```

In adding this print statement, we should see the following printed to the console:

```swift
backwardsFunction is called with Forever in Blue Jeans and Cracklin' Rosie
backwardsFunction is called with Song Sung Blue and Cracklin' Rosie
backwardsFunction is called with Song Sung Blue and Forever in Blue Jeans
backwardsFunction is called with Sweet Caroline and Cracklin' Rosie
backwardsFunction is called with Sweet Caroline and Forever in Blue Jeans
backwardsFunction is called with Sweet Caroline and Song Sung Blue
```

It's also possible to call on this `sort` function and instead of passing in a function that we've already created that meets its criteria, we can pass in the implementation right in line. What does that mean? What's the implementation? The implementation of our function really is just the `return a > b` - But `a` and `b` have been defined as `String`'s. How do we do this?

![](http://i.imgur.com/UKr428M.png?1)

If you begin to type out `.sort` on the instance of an `Array` you will be met with that ready to auto-complete for you, if you hit the return key, you should see the following.

![](http://i.imgur.com/vDvcjyk.png?1)

![](https://media.giphy.com/media/l0NwHXQy3kUSfFF60/giphy.gif)

This is **closure expression syntax**. 

![](http://i.imgur.com/7ypWjuW.png?1)

Lets name our parameters 

```swift
greatestHits.sort { (a: String, b: String) -> Bool in

    return a > b
    
}
```

We're providing this `sort` function with a function that has two arguments of type `String` and a return type of `Bool`. Except there's a few differences. 1- It doesn't have a name. There's no name to this function, we're defining it right in-line and passing it along to the `sort` function. Also, what's with that `in` keyword? That is the separation between the parameters and the actual implementation and use of those parameters. It can be helpful to think of those parameters getting used `in` the code which follows them. The code which comes after `in` is just like the code within the curly braces `{ }` of functions you've written in the past.


## Map

Here's a constant named `numbers` where we assign it a value of an array of `Int`'s. So the type of this constant is [`Int`].

```swift
let numbers = [1, 2, 3, 4, 5]
```

If we wanted to create a new array where we square  each value in this array, we know how to do that.

```swift
var squaredNumbers: [Int] = []

for n in numbers {
    
    let squaredNumber = n * n
    
    squaredNumbers.append(squaredNumber)
    
}

print(squaredNumbers)
// prints "[1, 4, 9, 16, 25]"
```

Lets do this another way using `map`.

If we begin to type out the `map` function available to all instances of an `Array`, we're met with the following function signature.

![](http://i.imgur.com/dPcXDaK.png?1)

The `map` function takes in one argument called `transform` of type `(Int) -> T`. Its return type is [`T`]. What is this type `T`? It's a generic type used by Swift. It can be _anything_. It can be a `Bool`, it can be your own custom type, anything.

So then how do we choose one? Well, if we were to implement a function in-line to supply to this `map` function - we can replace the `T` with whatever type we want to return, lets do that.

Here's what the code looks like when we auto-complete it when typing out `map`

![](http://i.imgur.com/TBP79aQ.png?1)

Lets now name our argument and replace the `T` with `Int` because we want to square our numbers so they will remain `Int`'s. 

```swift
numbers.map { (a: Int) -> Int in
    
    return a * a
    
}
```

We've passed in a function (that has no name) which has one argument called `a` of type `Int` which returns an `Int`. In our implementation we're utilizing the argument that would be passed into this function, `a` - squaring it by multiplying it by itself and returning that value to the caller of this function. 

Who is calling on this function with no name? `map` is - except we can't see that implementation because it's in the source files, but we're handing over to `map` which is a function itself this implementation and `map` can refer back to this implementation through its own argument `transform` which is the name of the argument (if you remember earlier) which will refer back to this implementation we provided.

Lets add a print statement to our implementation to see it getting called. How many times do you think it will get called?

```swift
numbers.map { (a: Int) -> Int in
    
    print("Our nameless function is getting called with \(a)")
    
    return a * a
    
}
```

```
Our nameless function is getting called with 1
Our nameless function is getting called with 2
Our nameless function is getting called with 3
Our nameless function is getting called with 4
Our nameless function is getting called with 5
```

Lets now store the value being returned by this `map` function to a constant and print it out to see that everything is working.

```swift
let newNumbers = numbers.map { (a: Int) -> Int in
    return a * a
}

print(newNumbers)
// prints "[1, 4, 9, 16, 25]"
```

## Filter

The `filter` function is available on any instance of an Array. 

Similar to the `sort` function, it takes in a function as an argument.

The return type of the `filter ` method is dependent upon the elements that make up the array for which you are calling the `filter` method on. Meaning, if it's an array of `Int`'s that calls the `filter` method, then its return type will be [`Int`] - an array of `Int`'s.

Lets create a constant called `grades` of type [`Int`] assigning it values that represent grades from a classroom.

```swift
let grades = [74, 92, 84, 50, 21, 50, 90, 100]
```

If we were to begin to type out the `filter` method available to any instance of an `Array` on this `grades` constant, we would be met with the following:

![](http://i.imgur.com/G1kwW8h.png?1)

The return type is [`Int`] - so calling on this particular instance method will return back to us an array of `Int`'s. The name of this instance method is `filter`, it takes in one argument called `includeElement` of type `(Int) -> Bool`. We know how to read that - it's a function that takes in one argument which is an `Int` and returns a `Bool`. 

This `filter` function will take the function `(Int) -> Bool` we give it and for each element in the array, it will call on our supplied function and only if it returns `true` will it store it in some `Array` (within its implementation) and then return back us that `Array` when it's done going through all of its elements.

Lets try it out.

Here is an function called `isGreaterThan85(_:)` that takes in one argument called `grade` of type `Int` and returns a `Bool`. That matches the `(Int) -> Bool` type to which `filter` is looking for. In our implementation, if the grade passed in is greather than 85, it will return `true`, if not it will return `false`.

```swift
func isGreaterThan85(grade: Int) -> Bool {
    return grade > 85
}
```

Lets now pass this into the `filter` method.

```swift
let goodGrades = grades.filter(isGreaterThan85)
print(goodGrades)
// prints "[92, 90, 100]"
```

We can also take advantage of being able to do all of this in-line without having to create a separate function.

```swift
let goodGrades = grades.filter { (grade: Int) -> Bool in
    return grade > 85
}

print(goodGrades)
// prints "[92, 90, 100]"
```

Doing it that way eliminates the need for us to create this separate `isGreatherThan85` function. It also becomes more readable (I know at first you might be like.. how is this more readable!) - but it keeps everything very close together so we can see exactly what this `filter` function is doing to create this new `goodGrades` array.

We can also add `print` statements to see when this getting called.


<a href='https://learn.co/lessons/MapFilterReduce' data-visibility='hidden'>View this lesson on Learn.co</a>
