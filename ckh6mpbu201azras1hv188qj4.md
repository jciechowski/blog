## String Calculator Kata in F#

Since  [@theimowski](https://twitter.com/theimowski)  introduced me to F# years ago, I wanted to try it someday. Finally, I feel like I'm ready for this challenge.
And following  [@swyx](https://twitter.com/swyx)  suggestion to learn in public, I'll try to explain each step in the form of a blog post.

I will start with one of my favorites, yet nothing complicated TDD kata -  [String calculator](https://kata-log.rocks/string-calculator-kata).    

This is a first part of a series:  

1. [String Calculator Kata](https://blog.ciechowski.net/string-calculator-kata-in-f) (this one)
1. [Another encounter](https://blog.ciechowski.net/string-calculator-kata-in-f-another-encounter)
2. [Fall of exceptions](https://blog.ciechowski.net/string-calculator-kata-in-f-fall-of-exceptions)



And now it's coding time!

## Adding single numbers

Following TDD rules, we will start with a test
```
[<Fact>]
let ``Passing empty value returns 0`` =
    let result = Add ""
    Assert.Equal(0, result)
```

We see that the surface of our public API is just one method `Add(numbers: string): int`
Our first test case verifies the addition of an empty string, which, of course, should be equal to 0.
Making our test pass doesn't require much brainpower:
```
let Add (numbers: string): int = 0
```

Moving forward, we can add a few new tests for single values. We also convert our test to _Theory_ in the process.
```
[<Theory>]
[<InlineData("", 0)>]
[<InlineData("5", 5)>]
[<InlineData("17", 17)>]
let ``Passing single number returns the same value`` (input, expected) =
    let result = Add input
    Assert.Equal(expected, result)
```

Of course, our current implementation isn't going to work. We can fix it with an elegant F# feature called pattern matching:
```
let Add (numbers: string): int =
    match numbers.Length with
    | 0 -> 0
    | _ -> int numbers
```

## Numbers with a delimiter
Let's try more complicated case. The next step is to add numbers separated by a comma.
First the test:
```
[<Fact>]
let ``Passing numbers with a seperator adds them`` () =
    let result = Add "1,2"
    Assert.Equal(3, result)
```

This time, we have to adjust pattern matching we used earlier
```
let Add (numbers: string): int =
    match numbers.Length with
    | 0 -> 0
    | 1 -> int numbers
    | _ -> numbers.Split([|','|], StringSplitOptions.RemoveEmptyEntries) 
            |> Array.map int 
            |> Seq.sum
```

Currently, we have three cases based on the length of our input.
1. 0 for the input of length 0, which means an empty string
2. returns the same value converted to integer when a single number was passed
3. And now the best part! We split the numbers by comma, map each number in a string format to integer and finally sum the result. We do it in full F# force using the pipeline operator, which feeds output from previous function to next one. Doesn't it look great?

Finally, we can add a few more tests, just to be sure:
```
[<Theory>]
[<InlineData("1,2", 3)>]
[<InlineData("5,8", 13)>]
[<InlineData("1,2,3,5,8,13", 32)>]
let ``Passing numbers with a seperator adds them`` (input, expected) =
    let result = Add input
    Assert.Equal(expected, result)
```

We implemented first two step from the Kata description. We can stop here for now. 
Full code is available on  [github](https://github.com/jciechowski/StringCalculatorKataFSharp).

In another episode, I will introduce custom delimiters.

