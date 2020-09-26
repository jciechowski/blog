## Interface with one implementation?
Give delegate a try!

Interfaces with only one method and one implementation always seem awkward to me. I felt it in my gut but wasn't able to articulate what's exactly wrong with them. Luckily, recently someone showed me another way of solving this issue.

## Fake abstraction
What's the deal with one method interfaces with only one implementation? Are they an abstraction or just a way to decouple the specific class from the "entry point"?

Undoubtedly, it's the latter option. 
Usually, when we have such code we don't have an abstraction, at least not a decent one.

But on the other hand, we want to use inversion of control. Because of this, it seems natural to extract the interface and register it in our IoC container.

Let's take a look at an example of infamous interface:
```
public interface IStationRepository
{
    IReadOnlyCollection<Station> GetAllStations();
}
``` 
  
And registration in the container:
```
services.AddSingleton<IStationRepository, StationRepository>();
```

That's the approach I would always encounter in the different codebases. What I don't like about it is that it defines an "abstraction" called *StationRepository *which does exactly one thing - returns all stations and has only one implementation.
How is it an abstraction when its behavior is so specific? 
Can we just drop the *I* prefix and use the implementation? All modern IoC containers can do that, why bother?

## Unit testing
Looking at the above code from a testing standpoint it's obvious that we need some way to decouple the implementation from the abstraction.
We don't want to make calls to the database from our super-fast unit test.

Following convention from the previous snippet, I would expect to see something like this:
```
public class StationRepositoryMock : IStationRepository
{
    public IReadOnlyCollection<Station> GetAllStations() => 
      new List<Station>
      {
          new Station("Gdansk"),
          new Station("Sopot"),
          new Station("Gdynia")
      };
}
```
And then inject *StationRepositoryMock *to classes that need stations.
So far, so good.
But what if we don't need this cumbersome approach?
## Delegates
You probably heard about delegates. If you ever did some WPF you can call yourself an advanced delegates user.

But how can a delegate type help us?
What if we defined our interface like that?
```
public delegate IReadOnlyCollection<Station> AllStations();
```
Wow, this looks good. No more strange interfaces, no more fake abstractions. Just one specific delegate with a precise and intent revealing name.
And here is the registration in the container:
```
services.AddSingleton<AllStations>(GetAllStationsImplementation);
```
You can then inject it into your queries or go crazy and make it static!
## Unit testing with delegate
How do the tests look with this delegate? As you probably expect, way better!
There is no need to create a class that implements an interface. You can prepare data in the arrange phase of your test, like so:
```
AllStations stationsMock = () => new List<Station>
{
    new Station("Gdansk"),
    new Station("Sopot"),
    new Station("Gdynia")
};
```
And then inject this delegate wherever you need it.
Whole code is available at  [github](https://github.com/jciechowski/InterfaceVsDelegate) 


How do you like it? Isn't that simpler and less cumbersome?




