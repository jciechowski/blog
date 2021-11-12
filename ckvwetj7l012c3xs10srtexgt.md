## Surround template for string interpolation in Jetbrains Rider

I dig string interpolation in C#. It is concise and easy to use. 
And even though JetBrains Rider is known for making developers super productive with all kinds of templates and shortcuts, there was one thing I've missed - the ability to surround a variable with string interpolation.

## Live templates
Why not create such a template by yourself?  
My problem was that I wanted to pass a stream name to the function but add some prefix to the stream beforehand.  
My code looked like this:
```
eventStore.SaveEvents(streamName, events, eventMaxAge)
```
After the change, it should look like this:
```
eventStore.SaveEvents($"rev-{streamName}", events, eventMaxAge)
```

I've found string interpolation a perfect solution for this issue. But after doing it time and time again, I've got tired of doing it manually. 
I wanted an easy way to surround stream name with string interpolation. 

## Using surround template

Then I've recalled that Rider has an option to surround a variable with brackets, for example.  
Let's create such a template for string interpolation!  
Try this sequence: double press shift, type live template, press enter, and voila!   
From this place, you can create a new template. 
The one I've created looks like this:


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636722757930/N8__2TslK.png)

How to use it?
Mark a variable you want to surround and press the shortcut set up for the "Surround with" command. In my case, it's ctrl+alt+j, but I've found my keyboard shortcuts relatively uncommon.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636723286303/tusFYaCgT.png)


After applying it using enter or pressing one, it will wrap the variable in a string interpolation like so:


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636723311812/1DfhR2AUo.png)

And that's what I need!


Templates are great for automating tedious typing. Who doesn't like IntelliSense tailored to their needs?