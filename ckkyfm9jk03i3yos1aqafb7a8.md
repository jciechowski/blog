## Using tuples for validations

Many functional techniques are proving their ground in C# codebases. One of my favorites is the `Result` type. The most basic form consists of a boolean flag indicating success and an array of errors. But in many cases, tuples are just enough. 

I fall in love with value tuples since their introduction in C# 7. They are perfect when you need a local data structure with a concise syntax.
One of the most common usages of this feature is validation. Add to this switch expression, and we have a great combo.

Validating PIN was never so easy:
```
(bool Success, string ErrorMessage) ValidatePIN(string pin) =>
    pin switch
    {
        null =>
            (false, "Missing value"),
        var p when p.Length > 4 =>
            (false, "PIN is too long"),
        var p when !int.TryParse(p, out _) =>
            (false, "PIN has to be a number"),
        _ =>
            (true, string.Empty)
    };
```
I'm a big fan of such an approach. I admit it was intimidating at first. But after a few tries of writing code in such a manner, I think this is the way.
