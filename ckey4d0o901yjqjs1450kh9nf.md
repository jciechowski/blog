## Highway from greenfield to legacy.

Apparently, everyone wants to work on greenfield projects. But just in a few months, your new shiny system can already be called legacy. Sounds ridiculous? Let me tell you a story. 

## The Dunning–Kruger peak
I remember that specific excitement when I moved from the company where we barely wrote any code to a brand new greenfield. Now I can really show my skills! Nothing will stand in my way to create a perfect product. 
 
As you may suspect, it didn't unfold that way. About 6 months later, I was already scared to touch some part of the system just before the release. I wasn't sure which modules I can break or even how I can test my changes manually. This is how we created a mythical monolith. At least we were able to deploy it to production. And it worked there for at least another 6 months.


## Senior style 
Skip forward a bit when I started to call myself a senior developer. 
Now I really know how to create systems from scratch! Decent architecture, domain knowledge, deep insight into the business - I got it! I was closely working with a more experienced architect, so I was pretty confident with our approach.
 
Sounds promising, right? Not really. After a bit more than 6 months, I was again afraid to make changes in some parts of the code. Even better - I was the owner of some modules that no one else except me could change. Reckon that's what makes you a senior 😅 
To add insult to injury system isn't yet running on production. 

## Embrace the legacy

What these two projects have in common? Why are they so similar, although from a completely different domain? 
 
First and foremost, both projects didn't have clear business goals. One was "just a simple portal," and the other was a replacement for an old system. But that's a story for another time. 
 
At least from my perspective, the worst part was a horrendous amount of technical debt that manifested in different aspects. We barely wrote any tests. We didn't document business knowledge in any way. We spoke a foreign language than people who ordered and were going to use those systems.
 
We didn't feel like any of these issues were especially important. But when they add up, you land in a legacy land. And that's definitely not something you want, I think we can agree on that.
 
So, what's the formula for fast-track your greenfield into legacy?
Don't write too many tests. Especially integration one.
Don't evolve and agree on a common language. It's better to have one for the business and one for the code.
Don't try to overthink the domain. We are agile; we can fix it as we go.
 
Guess greenfields are not as easy as one could think.
And definitely, they are not my favorite kind of systems.