## Partial application using delegates in C#

I'm a big proponent of using delegates instead of interfaces as described [here](https://blog.ciechowski.net/interface-with-one-implementation-give-delegate-a-try).
Recently I've found out that it works wonderfully with the partial application!

Partial application is a concept widely used in functional programming. It's a function that has parameters already baked into it.  
Don't worry if this sounds strange. I hope the example will clear up everything.

When using a DI container, it's common to pass a dependency to our class only to pass it down to some other service. It leads to coupling and no clear separation of responsibilities.  
What if we could pass a function that has all the dependencies already baked into it? That's precisely how partial application works!

First, create a delegate. It's an abstraction we will pass around. 
```
delegate string ReadById(string id);
```
Next, we create an implementation.
```
static class DatabaseReader {
    static string ReadById(Database db, string id) => db.Read(id);
}
```

Registration is the most tricky part.
```
builder.Services.AddSingleton<Database>();
builder.Services.AddSingleton<ReadById>(x => 
  id => DatabaseReader.ReadById(x.GetRequiredService<Database>(), id));
```
Let's stop for a moment and dissect this construct.
First, we register a database using the usual approach.
Subsequently, we register the `ReadById` delegate using a partial application. 
We can use it like so:
```
class ServiceUsingDelegate {
    readonly ReadById _reader;

   public ServiceUsingDelegate(ReadById reader) => _reader = reader;

   public string DoingWork(string id) => _reader(id);
}
```
Anytime we call the delegate, under the hood container will inject the database into it. We don't have to care about its dependencies at all!
How cool is that?

It allows our service to be solely focused on its task. We don't have to pass any extra params like a logger or mapper into it. We only use the specific function.

I've found such code in one of our projects recently and instantly fell in love with it.

What do you think about it?


