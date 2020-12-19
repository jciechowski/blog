## Tuple vs multiple params in F# functions

In my mind, the F# code was almost braceless. I enjoyed reading it because it felt so natural.  
Recently, I wrote a bit of F# code. And guess what? There were way too many braces for my taste.  
Take a look at this code:
```
let extractNumbers (numbers: Input, delimiterType: DelimiterType)
let numbers = extractNumbers (input, delimiterType)
```
Braces all over the place! How did it happen? Was my initial feeling wrong?

Not at all. I've used it wrong. I didn't know how to declare a function with multiple params. Instead, I've defined a tuple as an input.  
An improved version could look like this:
```
let extractNumbers (numbers: Input) (delimiterType: DelimiterType)
let numbers = extractNumbers input delimiterType
```
Can you spot the difference?   
I've changed the definition of the function. Instead of a tuple, now it takes two parameters. I've only skipped coma between the params. And now I feel much better about this code. 

