## Why you need a domain model?

As described  [by Martin Fowler ](https://martinfowler.com/eaaCatalog/domainModel.html), the domain model pattern is just data with behavior treated as one object. I found that majority of developers associate it inherently with Domain Driven Design.
It is unfortunate. It can work very well on its own without other patterns from the blue book. And because of all myths around DDD, it is used way too rarely.

## The anemic model
The "classic" school taught us that our models should never have any behavior, just pure data. In C# it manifests in classes with only properties and nothing more. This approach is called the anemic model.
But it doesn't mean that our objects do not have any behavior. It just means that we can't put it in the same place as our data.

Which leads to a question: where can we define methods that describe the behavior of our domain?
From my experience, there is only one, very disappointing answer - everywhere!
Sometimes it's in the services. Sometimes it's in the extension methods. Sometimes it's in another assembly because, you know, that improves modularity. Every place is fine as long as it follows our team's "best practices." Of course, except the model itself.

## So, why you need a domain model?
You will have to keep that behavior somewhere. Why not try to model the domain that way? Let's use the business language. It will make your domain easier to understand for developers. It will also keep all that knowledge in one place, making the whole system more maintainable.
Scattering all those pieces of information in different services works in the opposite direction.

Another thing is that it makes testing incredibly easier. Trying to test a business process that spans around a few services and database calls is painful.
You can put that process in the domain model without external dependencies. Then write a unit test for this, like a champ.
It will also make your handler/controller much cleaner.

Next time you start a greenfield project, give yourself a few hours for domain modeling, and seriously consider the rich domain model.