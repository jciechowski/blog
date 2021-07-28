## Await tasks with multiple result types

Regularly I want to await a few tasks and get their result. Surprisingly, there is no obvious way to do that. _Task.WhenAll_ can be helpful, as we see later, but it doesn't solve the problem.  
My common scenario looks like this:
```
var dbTasks = getDataFromDbTasks();
var otherServiceTasks = GetDataFromServiceTasks();

var dbData = await dbTasks;
var otherServiceData = await otherServiceTasks;
```
I found this approach very verbose. I prefer to await all the tasks at the same time.
That's why we have an extension method in our codebase specifically for this purpose:
```
public static class TaskAwaiter {
    public static async Task<ValueTuple<T1, T2>> AwaitAll<T1, T2>(
        Task<T1> x,
        Task<T2> y) 
            => (await x, await y);
}

```
Such AwaitAll method simplifies the above code to this:
```
var dbTask = getDataFromDbTask();
var otherServiceTask = GetDataFromServiceTask();
var (dbData, otherServiceData) = 
    await TaskAwaiter.AwaitAll(dbTask , otherServiceTask);
```
That's precisely what I wanted! So far, so good.

But there is one caveat. What if I have a lot of tasks to await? I don't want to add another extension with more and more parameters.

Here _Task.WhenAll()_ method becomes handy. It creates a single task that completes when all the other supplied tasks have been completed.  
 
Usage looks like follows:
```
var dbTask = getDataFromDbTask();
var singleTaskFromCollectionOfTasks = Task.WhenAll(CollectionOfTasks());

var (dbData, singleTaskData) = 
    await TaskAwaiter.AwaitAll(dbTasks, singleTaskFromCollectionOfTasks);
```

Armed with these two methods, I can quickly write meaningful and readable code.

