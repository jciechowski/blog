## Http call using F# and HttpClient

It took me a few tries to finally call an HTTP endpoint using F#.  
I've ended up with this code:
```
let result =
    async {
        let client = new HttpClient()
        let url = "https://api.frankfurter.app/latest?amount=10.99&from=USD&to=PLN"

        let! result =
            client.GetFromJsonAsync<ApiResponse>(url)
            |> Async.AwaitTask

        printfn $"10.99 in USD is {result.Rates.PLN} in PLN"
    }

result |> Async.RunSynchronously
```
For my taste, it is very similar to the C# code.  
The most significant difference lies in the async block.
It doesn't run until you do it explicitly!   
In the C#, when you create a task, you have no control over when it starts. I've described one of such scenarios [here](bloghttps://blog.ciechowski.net/creating-tasks-in-batches).

Only after calling _Async.RunSynchronously_ the actual workflow runs. It reminds a bit of _await_ instruction in C#, but the inner working is different.

The complete code is available on [gist](https://gist.github.com/jciechowski/0397ef73f889d7a652eb060bd51880d4).

I like this functional approach with cold workflows. It seems more intuitive to me. One downside I can see is that it may require more code. But I don't have enough experience with F# to be sure.

