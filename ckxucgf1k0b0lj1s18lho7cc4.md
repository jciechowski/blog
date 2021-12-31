## Modeling exceptions

Modeling exceptions is hard. On the surface, it seems easy. Everyone knows what an exception is, right? But when you think deeper about it, there are a lot of traps that you can fall into. 

## Throwing an exception is a goto
Exceptions are hidden goto statements. That's one of the most significant troubles with them.  
Whenever you notice a method that throws an exception and catches it in another place, stop for a moment. Such early returns make it hard to resonate about program control flow and make code difficult to refactor.

## Don't use exceptions for validation
Very often, we use exceptions for validation.  
Someone called your method with an empty string? Throw an exception!  
Start date is older than the end date? Throw an exception!  
Height is a negative number? Throw an exception!    


This doesn't mean that exceptions can't be used for validation.
For example, they are excellent when verifying input to the web API.  
  
But when working with business logic, take a look at different techniques:  
* [Type Driven Programming](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/)
* [Result](https://fsharpforfunandprofit.com/posts/elevated-world-3/#example-validation-using-applicative-style-and-monadic-style) types widely used in functional programming.

## When to throw
Having all that behind us, how do I know that specific situation should be modeled as an exception?  
These questions can be helpful to ask:
* Do you know what to do when the case that seems like an exception occurs?
* Is this exception a part of your domain?  

If the answer to the first question is positive, just log the issue and move on.  
The second question is more subtle and requires a bit of thinking.
 [Scott Wlaschin has an excellent post on this issue](https://fsharpforfunandprofit.com/posts/designing-with-types-making-illegal-states-unrepresentable/)  

## Throw in an elegant way
Finally, if you throw an exception, make the reason obvious.  
Name it correctly using language from your domain. Default exceptions are rarely a good idea.   
And remember that exceptions are written for other developers. Therefore, there is nothing wrong with keeping it technical.


