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

Who doesn't love Neil Diamond? A pre-requisite to this lesson might sound a little crazy, but it's a must. Put some headphones on and listen to some of Neil Diamonds greatest hits. You can start with [Soolaimon](https://itun.es/us/-oOkgb?i=1177192316) if you like. But this is a requirement as you read through this reading. Feel free to exit your chair and dance a little (the dancing part isn't a requirement). 

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

## Reduce

A Song of Ice and Fire is considered one of the greatest stories ever told (and it's not done being told!). Most of you might refer to this series as "Game of Thrones" which has become a very popular TV show on HBO.

![](http://i.imgur.com/BURdUwq.jpg?1)

As it currently stands (as of Jan. 4th, 2017), there are five books in the series. Five books with _a lot_ of pages. What do pages have?.. words! There are a LOT of words in this series. Lets break it down per book:

* A Game of Thrones: 298,000
* A Clash of Kings: 326,000
* A Storm of Swords: 424,000
* A Feast for Crows: 300,000
* A Dance with Dragons: 422,000

Lets throw these figures into an `Array`.

```swift
let wordsPerBook = [298000, 326000, 424000, 300000, 422000]
```

If we wanted to calculate the _total_ amount of words in the entire series, we could do something like this:

```swift
var total = 0

for words in wordsPerBook {
    total += words
}

print(total)
// Prints 1770000
```

That's a lot of words, nearly 2 million. According to George R.R. Martin, there's two books left in the series which will definitely put us over 2 million words. I remember finding a hard time writing 250 words for an essay due in high school.

Notice how we created an initial starting point for us here, with a variable `total` with an initial value of 0. `total` here is a variable of type `Int`. Next, we created a for-in loop which is looping through our `wordsPerBook` `Array`. Each time through this for-in loop, we are increasing our `total` variable by the value of `words` (in its current iteration). It will iterate through this `Array` 5 times. When its done going through this for-in loop, we're printing our `total` variable which prints `1770000` to the console.

Starting with an `Array` which contains multiple values (like our example above), and ending with _one_ value (like our example above) where we get down to this one value by iterating through an `Array` is something you might come across when creating your application.

It's common enough to where Apple has a method available to `Array`s similar to the methods you were exposed to in this reading above.

This method is `reduce`. The method signature is `reduce(_:_:)` as it takes in two arguments. It also returns back a value. At first glance, it seems as if there's a lot going on with this method.

If we were to command click the `reduce(_:_:)` method in our Playground file, we would be met with the following:

![](https://s3.amazonaws.com/learn-verified/ReduceScreenF.png)

Woah, that is some method signature. Don't be too freaked out by it, we will break it down.

The word `Result` here is a generic type. Meaning, it can be _anything_. It's not a specific type (yet). But when we choose what the type of `Result` should be (when we call on this method), wherever we see the `Result` word, it needs to all be replaced with the same type. What does that mean? If we decide to to replace the word `Result` with the type `Int` then everywhere we see the word `Result`, it needs to be replaced with the `Int` type, we can't have one `Result` word be the type `Int` and the other `Double`. They need to match.

A lot of words. But stay focused, we're almost there. How do we replace the word `Result` as it's being used here (which is four times) with a type. Well, the compiler is able to infer the type we want to use and will replace this word automatically for us. Here's an example:

```swift
let sum = wordsPerBook.reduce(0, { (runningTotal: Int, next: Int) in
    
    print("runningTotal: \(runningTotal)")
    
    print("next: \(next)")
    
    return runningTotal + next

})

/* Prints
 runningTotal: 0
 next: 298000
 
 
 runningTotal: 298000
 next: 326000
 
 
 runningTotal: 624000
 next: 424000
 
 
 runningTotal: 1048000
 next: 300000
 
 
 runningTotal: 1348000
 next: 422000
 */
```

The first argument we provided to this method is the `Int` value 0. The next argument we provided to this method is _another_ method. Using trailing closure syntax, we're passing the following function along as an argument:

```swift
{ (runningTotal: Int, next: Int) in
    
    print("runningTotal: \(runningTotal)")
    
    print("next: \(next)")
    
    return runningTotal + next

}
```

This anonymous function we've created doesn't have a name. It takes in two arguments and returns a value. The type of this function we have to pass to it is (`Result`, `Element`) -> `Result`. Since we've already established that the first argument of the `reduce` function is of type `Int` (because we've given it the number 0). That means that wherever we see the word `Result`, it is instead now of type `Int`. And `Element` represents the type of the elements within the `Array` we're calling this method on. The type of the elements in the `Array` we're calling this `reduce` method on are _also_ of type `Int`. Which means the type of this method we need to pass along to the `reduce` method is of type: (`Int`, `Int`) -> `Int. This matches what we did above! Know that when creating an anonymous function using trailing closure syntax, we don't need to specify the types nor the return type.

OK. So we understand that this `reduce` method is given two things. When called on, we're giving it the value 0 and a function which (besides the two print statements) returns back `runningTotal` + `next` which are the names of the two arguments of the function we're handing over to `reduce`.

`reduce` takes these two items and does something with it. As you can see, it's calling on the anonymous function we give to it over and over. If you step through the print statements, you can see _exactly_ what the `reduce` function is doing with it. It's first calling on the method we give it, passing in the initial value (which is the 0) and then the first element in the `Array` which `reduce` is being called on. Our method adds these two items together then it returns it. Then you can see that our method gets called again, this time the `runningTotal` variable is now equal to what was just returned prior (the value being 298000). The `next` variable is now equal to the second item in the `Array`. This process continues until it iterates through the entire `Array`.

As you can see this works exactly as our prior version before. One advantage to doing it this way is that we can use the `let` keyword to have `sum` be a constant that can _never_ be changed. In our prior example, `total` had to be a variable (declared with the `var` keyword) thus making it mutable. 

It's not easy just reading how a function like this works. You need to test it out by writing your own code. Open up a playground file and make an attempt at calling on `reduce` on your own `Array` instance. In doing so, it should click for you. Also, watch the video I have linked above which walks you through how `reduce` works.





<a href='https://learn.co/lessons/MapFilterReduce' data-visibility='hidden'>View this lesson on Learn.co</a>
