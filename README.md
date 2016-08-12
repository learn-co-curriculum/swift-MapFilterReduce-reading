# Sort, Map, Filter & Reduce



> I like turtles. 
 





# Cool Things With Functions

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

!](https://media.giphy.com/media/l0NwHXQy3kUSfFF60/giphy.gif)

This is **closure expression syntax**. 

![](http://i.imgur.com/7ypWjuW.png?1)

Lets name our parameters 

```swift
greatestHits.sort { (a: String, b: String) -> Bool in

    return a > b
    
}
```

We're providing this `sort` function with a function that has two arguments of type `String` and a return type of `Bool`. Except there's a few differences. 1- It doesn't have a name. There's no name to this function, we're defining it right in-line and passing it along to the `sort` function. Also, what's with that `in` keyword. That is the separation between the parameters and the actual implementation  and use of those parameters. Think of it as the curly braces `{ }` when you create a function as you normally have in the past.


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





<a href='https://learn.co/lessons/MapFilterReduce' data-visibility='hidden'>View this lesson on Learn.co</a>
