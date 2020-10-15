## Mutation testing with Stryker .Net

What you do when trying to measure the quality of your tests? Neither code coverage nor branch coverage won't help.

Maybe tackle this from a different perspective. What if one could change our production code and see how tests behave? If they are at least on a decent level they should catch such change and fail, right?
That is how mutation testing was born.

Mutation testing isn't new. But it's not very popular in the .Net world.
The idea behind it is simple. First, it introduces some changes to the production code and then runs the tests against it. Such a transformation is called a mutant.
If our test results turned red, this means the mutant survived. When they stayed green, as it should be, the mutant was killed.

To let our mutants loose and verify the quality of our tests we will use stryker .Net (https://github.com/stryker-mutator/stryker-net).

To install it just run dotnet install stryker (.Net core is required) and you are good to go.
Then, run dotnet stryker from the test project directory and wait for the results.

When Stryker is running, you should see a lot of different pieces of information. For example the number of your test, the number of generated mutants, and progress.
Finally, you should see a path to the HTML report. Let's see what we got in here.

I've run it against this project: https://github.com/jciechowski/VendingMachineTDDKata
The overall mutation score is decent at 87.5%. It means that 87,5% of all mutants have been killed.

What were the changes that slipped between tests?
We can see two meaningful mutations in GetProductWithChange class.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602790066140/3Gg2qpemW.png)

The first one means that Stryker changed the comparison operator from greater to greater or equal. We can skip this one because we cover the case of an equal price a few lines above.

The second change is about switching decrementation to incrementation.
Oh, this one is interesting. It means that we can increment the number of our snacks after selling it. 
It sounds like a recipe for disaster, doesn't it? And it's getting even worse. Our tests didn't catch this mutant!

What can we do about it? We have to verify that after selling a product number of available items is decreased. Simple as that.

As you can see mutation tests offer us a very different approach to test quality. They simulate introducing bugs and tells us if we can catch them with our tests suite. The caveat is that they are rather slow. This can be easily resolved by running them on for example night build.

