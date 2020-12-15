## Emberace MemberData attribute.

Naming is hard. Well described test suites are even harder. I've seen a lot of tests that goes like this:
```
[MemberData(nameof(Data))]
public void EmailValidator_Should_Validate_Correctly(
    string data, bool expected)
{
  var emailValidator = new EmailValidator();
  var result = emailValidator.Validate(data);

  Assert.Equal(expected, result);
}
```
As you may expect, I wasn't happy with this boilerplate code as well.  
But let's focus on something else. I see a few issues with the name of the test:
* it doesn't describe any business behavior
* it's hard to read because it doesn't follow English grammar
* it includes the name of the service  

Sadly, this is the desired template of any test you wrote in that specific codebase.

How can we convey any meaning in such a situation?
Embrace the MemberData attribute!

Usually, our data collection has a few entries. It can look like this:
```
public static IEnumerable<object[]> Data =>
    new List<object[]>
    {
        new object[] {
            "donatello@example.com", true,
            "donatello.turtles@example.com", true,
            "leonardo@examplecom", false,
            "leonardo.example.com", false
        }
    };
```
Do you see the pattern? Try to make use of it and split the entries.  
Divide it  as follows:
```
public static IEnumerable<object[]> ValidEmails =>
 new List<object[]>
 {
   new object[] {
    "donatello@example.com", true,
    "donatello.the.turtle@example.com", true,
   }
 };
public static IEnumerable<object[]> InvalidEmails =>
 new List<object[]>
 {
   new object[] {
    "leonardo@examplecom", false,
    "leonardo.example.com", false
 }
};
```
Follow by the test:
```
[MemberData(nameof(ValidEmails))]
[MemberData(nameof(InvalidEmails))]
public void Should_Validate_Correctly(string data,bool expected)
{
    var emailValidator = new EmailValidator();
    var result = emailValidator.Validate(data);

    Assert.Equal(expected, result);
}
```
Of course, one could argue that it won't change much. We can't see that modification in a test runner window. And that's true. We have to open the file and read its content anyway. But it's a step in the right direction.




