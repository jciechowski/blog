## String Calculator Kata in F# - another encounter

So far, we can only add numbers delimited by a comma.
The plan for today is to implement step 3,4 and from the [description ](https://kata-log.rocks/string-calculator-kata), which is

- handle new line "\n" as a delimiter, in addition to comma
- support custom delimiters passed in input.
- Besides, we will do some refactoring after making everything work.


This is a second part of a series:  

1.  [String Calculator Kata](https://blog.ciechowski.net/string-calculator-kata-in-f ) 
2.  [Another encounter](https://blog.ciechowski.net/string-calculator-kata-in-f-another-encounter) (this one)
3. [Fall of exceptions](https://blog.ciechowski.net/string-calculator-kata-in-f-fall-of-exceptions)
4. [Happy end](https://blog.ciechowski.net/string-calculator-kata-in-f-happy-end)


## Additional delimiter
Obviously, we start with a test:
```
[<Fact>]
let ``Passing numbers with a newline separator adds them`` () =
    let result = Add "1\n2,3"
    Assert.Equal(6, result)
```

The fix for this failing test is straightforward. Add "\n" as an additional delimiter.
Now Add function looks almost the same:
```
let Add (numbers: string): int =
    match numbers.Length with
    | 0 -> 0
    | 1 -> int numbers
    | _ ->
        numbers.Split([| ','; '\n' |], StringSplitOptions.RemoveEmptyEntries)
        |> Array.map int
        |> Seq.sum
```
## Custom delimiter passed by the user
As we expand the functions of our String Calculator we have to support any single character delimiter, for example, a colon.
Here is the test:
```
[<Fact>]
let ``Passing numbers with a custom delimiter adds them correctly `` () =
    let result = Add "//;\n1;2"
    Assert.Equal(3, result)
```

As you have noticed, the code for all the tests is the same.
The only difference lies in the data we pass into it. Why not merge all of it into one test and add a new InlineData attribute for any other case? It may look like a good idea, generalize all the things!

But I'm doing that on purpose. I want our tests to serve as documentation. Splitting them into one specific use case allows me to do that. I'm not a fan of relying on data to describe behavior.
There is one trick, though. You can use MemberData attribute, but that's a story for another time.

Getting back to the point, let's make our test pass.
Add function takes rather simple shape:
```
let Add (numbers: string): int =
    (extractNumbers numbers, extractDelimiter numbers)
    |> sumNumbers
```

Let's take a glance at extractNumbers function which presents as follows:
```
let extractNumbers (numbers: string) =
    let startsWithCustomDelimeter = numbers.StartsWith "//"
    if (startsWithCustomDelimeter) then numbers.[4..numbers.Length] 
	else numbers
```
We can see two branches. The former takes care of a case when we have a custom delimiter. When this part is called, our input looks like this "//;\n1;2"
The latter case is the simplest one with a single number or a default separator.
ExtractDelimiter is more interesting:
```
let extractDelimiter (numbers: string) =
    let defaultDelimiters = [| ","; "\n" |]
    let startsWithCustomDelimiter = numbers.StartsWith "//"

    if (startsWithCustomDelimiter ) then
        let customDelimiter = [| string numbers.[2] |]

        Array.concat [| defaultDelimiters
                        customDelimiter |]
    else
        defaultDelimiters
```
Stay with me! It's not that scary. When we find a custom delimiter, we concatenate it with a standard one - a comma and a newline and return such array. 
We didn't find a custom delimiter? Just return a default delimiters.
Steps 3 and 4 finished! Full code is available on [github](https://github.com/jciechowski/StringCalculatorKataFSharp/commits/step-3-and-4). 
Now it's time to add some functional look and feel.
## Refactor
I won't describe every line I've changed. I will explain the rules that I've followed when doing the refactor. 
- Get rid of ifs and use pattern matching, even for booleans
- Introduce simple types for a better domain modeling
- Use pipes and composition as often as possible. We used pipelining before the refactor anyway. I was able to add some composition, though.

And that's all. A more exhaustive list can be found at  [Scott Wlaschin](https://twitter.com/ScottWlaschin)  [blog](https://fsharpforfunandprofit.com/learning-fsharp/#dos-and-donts).
Rest of the code resides on  [github](https://github.com/jciechowski/StringCalculatorKataFSharp/commits/refactor).

I feel like that's enough for today's episode. Next time we will look at railway oriented programming and implement a few more steps.






