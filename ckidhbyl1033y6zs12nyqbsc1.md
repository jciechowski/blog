## String Calculator Kata in F# - happy end

Our calculator works pretty well so far. We were also able to use a few functional techniques in the process. Railway programming and composition is one of them.
  
Today we are going to focus on different types of delimiters:
* a delimiter can be of any length
* user can pass multiple delimiters of any size as an input
 
We are going to write a bit more code to implement these cases. Furthermore, we will do some domain modeling.

As always, in the beginning, there is a test:
```
[<Fact>]
let ``Delimiter can be of any length`` =
    let result = Add "//[***]\n1***2***3"
    Assert.Equal(Ok 6, result)
```
The first thing we will do is introduce a new type called DelimiterType
```
type DelimiterType =
    | Default
    | OneCharacterLong
    | MoreThanOneCharacterLong
```
We model our delimiter in a more granular way and explicitly define all available options with this type. Which, in turn, improves our domain.

With the introduction of a different kind of separators, we have to refine our algorithm. Instead of only deciding if a custom delimiter was passed, we now have to recognize its type. We do that with a new function:
```
let findDelimiterType (numbers: Input) =
    match numbers.StartsWith "//" with
    | false -> Default
    | true ->
        match numbers.Contains "[" && numbers.Contains "]" with
        | false -> OneCharacterLong
        | true -> MoreThanOneCharacterLong
```
I'm not satisfied with this approach. This double pattern matching seems clumsy. But I don't know what to do with it, and it works. Refactoring remarks are more than welcome.
 
It also forces us to add a new case in both extractNumbers and extractDelimiter:
```
let extractNumbers (numbers: Input, delimiterType: DelimiterType) =
    match delimiterType with
    | Default -> numbers
    | OneCharacterLong -> numbers.[4..numbers.Length]
    | MoreThanOneCharacterLong ->
        let delimiterEnd = numbers.IndexOf "]" + 2
        numbers.[delimiterEnd..numbers.Length]

let extractDelimiter (numbers: Input, delimiterType: DelimiterType) =
    let defaultDelimiters = [| ","; "\n" |]

    match delimiterType with
    | Default -> defaultDelimiters
    | OneCharacterLong ->
        let customDelimiter = [| string numbers.[2] |]

        Array.concat [| defaultDelimiters
                        customDelimiter |]
    | MoreThanOneCharacterLong ->
        let delimiterStart = numbers.IndexOf "[" + 1
        let delimiterEnd = numbers.IndexOf "]" - 1

        let delimiter =
            [| numbers.[delimiterStart..delimiterEnd] |]

        Array.concat [| defaultDelimiters
                        delimiter |]
```
One more change in parseInput function, where we extract delimiter type: 
```
let parseInput (input: Input): InputWithDelimiters =
    let delimiterType = findDelimiterType input
    let numbers = extractNumbers (input, delimiterType)
    let delimiters = extractDelimiter (input, delimiterType)

    (numbers, delimiters)
```
And all our tests pass!

There is only one more requirement left. It's passing multiple separators of any length at once. Translate this into a test:
```
[<Theory>]
[<InlineData("//[*][%]\n1*2%3", 6)>]
[<InlineData("//[!!][..]\n1!!2..3", 6)>]
let ``Multiple delimiters can be passed at once`` (input, expected) =
    let result = Add input
    Assert.Equal(Ok expected, result)
```
Similar to the previous step, we have to change extracting numbers and delimiters functions. 
Luckily, we don't have to change a lot of code. Working code looks in extracting delimiter looks like so:
```
| MoreThanOneCharacterLong ->
    let delimitersStart = numbers.IndexOf "["
    let delimitersEnd = numbers.IndexOf "\n" - 1
    let delimiters = numbers.[delimitersStart..delimitersEnd]

    let customDelimiters =
        delimiters.Split([| "["; "]" |], 
                          StringSplitOptions.RemoveEmptyEntries)

    Array.concat [| defaultDelimiters
                    customDelimiters |]
```
and extracting numbers:
```
 MoreThanOneCharacterLong ->
    let delimiterEnd = numbers.LastIndexOf "]" + 2
    numbers.[delimiterEnd..numbers.Length]
```
And that's all! String calculator kata in F# is done! Full code is on my  [github](https://github.com/jciechowski/StringCalculatorKataFSharp). 

I have to say I've enjoyed it a lot. There were times when I was stuck even though this kata looks relatively simple, right?
F# also seems like an interesting language. Not only because of a different paradigm but also for being succinct. I've also loved modeling with types. For a C# developer, it's just so easy. 
 
The best part? It only made me more hungry! Expect more F# code from me. If you have any ideas about the next steps send me a tweet.