## Writing business oriented tests

It took me years to figure out what was wrong with writing tests against the implementation.   
I knew the theory. Focus on the business-specific behavior and write tests before an actual code.   
But it was hard. I never understood how I could write tests before code. Are an existing solution not needed for the tests to make sense?  

Spoiler, it's not. If you think about the system as a product that has to deliver value, thinking about test scenarios ahead of the solution is more manageable.  

Imagine the following scenario where we accept a file through an API and want to parse it and read data.  
From here, we can take two paths.   
1. First one, just do it! Take an expected file, validate it using some method we wrote and then read the data because now we know that file is correct.
2. The second one, think before doing. 
Ok, we have to read a file. What file can we expect? Is the format fixed? What are we going to do in case of errors? How often will we receive such a file? The list goes on.

### The lazy way
Using the first approach, we end up with the following test scenarios:
- When uploading a file, it should store the document
- When uploading a file, it contains duplicates; it should store only distinct documents
- When uploading a file with only not parsable rows; it should not be stored
- When uploading a file and it is not CSV text content type, API should return a bad request

It doesn't sound like a description of business behavior. Furthermore, there are many technical details here, like API and parsable rows. It requires a decent knowledge of the inner workings of the code to understand what's going on.

### The product way
Let's take the second path and start by describing expected behavior.

We know that we will receive a file. The file has to be in a specific format, CSV, in our case. It will contain errors, but we can't foresee all kinds of mistakes. When the file is ok, we store it in the database. Who can send us a file?

Now, we can state the problem in the form of test scenarios:
- Receiving a file with errors in the content should create a summary of mistakes. Going further, how do we handle different kinds of problems?
- Discard the empty file.
- The correct file should be stored in a database.
- An unauthorized user sends a file.

I found these scenarios more related to the real problem we are trying to solve. I don't need any knowledge about the details of the implementation. I  want to know if we modeled the process correctly.   
Additionally, using such scenarios, we can find gaps in our understanding.

### It takes time
Taking a more product-oriented approach is a skill that took me years to develop. It helped me in other areas and made me a better software developer. 

Remember, part of the system you are working on is a more significant piece of the business process. Focus more on the business, and you will gain a lot.




