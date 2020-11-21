## String Calculator Kata in F# - fall of exceptions

Today we are going to implement handling negative values and values that are greater than 1000. It's step 5 and 6 from the kata description:
* calling Add with a negative number will throw an exception "negatives not allowed" - and return passed negative values
* numbers greater than 1000 are ignored, so 1002 + 1 equals 1

Finally, we can try to handle exceptions in a functional way.  
Spoiler alert! There won't be any exceptions.

Since last time we have an elegant pipeline. 
```
let sumNumbers = parseInput >> convertToNumbers >> Seq.sum
```
Because we use composition, we can pass the data from one function to another. Additionally, it makes it open for extension by adding a new function to our current flow.

Back to handling negative values, we start with a test:
```
[<Fact>]
let ``Cannot add negative values`` =
  let result = Add "-3, 1"
  Assert.Equal(Error "-3", result)
```
When adding a negative value we expect that we get that value back wrapped in an `Error`.
That strange prefix inside the assertion can be surprising, right?
It means an error, obviously, but what kind of error is this?  
That is my friends, an elegant way of handling exceptions. Without throwing an exception at all!  

Take a look at Add function:
```
let Add (numbers: string): Result<int, string> =
    sumWithFilter numbers
```
Notice the return type of the function. From `int`, it turned to `Result<int,string>.` Why? Because now it can return one of two values. An integer when everything went fine or a string when we tried to add negative values. 

Stop for a moment here. What's wrong with throwing an exception? Everyone does that all the time! Why can't we follow the same path?

But is it an exceptional situation? I don't think so. It's a part of our domain!
We know that we will deal with a negative value. Also, we know how to handle such a case. Let's make it explicit with the `Result` type.  
By using this approach, we make it obvious that our function can have two mutually exclusive outputs - one for success, and one for an error.
You can read more on this on a great [@ScottWlaschin](https://twitter.com/ScottWlaschin) [blog](https://fsharpforfunandprofit.com/posts/recipe-part2/
).  
Having the `Result` behind us, we can attach to our pipeline a filter for negative values like so:
```
let sumWithFilter =  
    parseInput  
    >> convertToNumbers  
    >> findNegativeValues  
    >> sum
```
and findNegativeValues implementation:
```
let findNegativeValues (numbers: Numbers) =
    let negativeValues = Array.filter (fun x -> x < 0) numbers
    match Array.isEmpty negativeValues with
    | true -> Ok numbers
    | false -> Error negativeValues
```
That is the place where we elevate our pipeline into the `Result` world.
When we got any negative numbers in the input we return an `Error` and said values. In the other case, we return an `OK` of input.

Because of elevating to the `Result` land, we have to modify our sum function:
```
let sum (numbersWithNegatives: Result<Numbers, int[]>) =
    match numbersWithNegatives with
    | Ok n -> Ok (Seq.sum n)
    | Error e -> e |> Array.map string |> String.concat "," |> Error
```
Notice the pattern matching here. If we got only positive values we just return an `OK` of summed numbers. When we got any negative values we return an `Error` with concatenated negative values.

This way we solved the issue with negative values. We didn't use any exceptions but achieved the same result. Key point is that our `Add` function now returns an `Ok` or an `Error`

We have one more thing to do. It's step 6 and ignoring numbers bigger than 1000.
This one is fairly easy. We have to filter out any number that is greater than 1000 with this function
```
let ignoreValuesGreaterThan1000 (numbers: Numbers) =  
    Array.filter (fun x -> x < 1000) numbers
```
And add it to our pipeline
```
let sumWithFilter =  
    parseInput  
    >> convertToNumbers  
    >> ignoreValuesGreaterThan1000  
    >> findNegativeValues  
    >> sum  
```

Full code is available, as always, on my  [github](https://github.com/jciechowski/StringCalculatorKataFSharp/tree/step-5-and-6).

In the next part, we will implement all three remaining steps.
See you there!

