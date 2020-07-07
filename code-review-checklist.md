# Code Review Checklist

This is a checklist I created for Waverley Software and we published it [on our website](https://waverleysoftware.com/blog/code-review-best-practices/)

During code review, you should pay attention to all these aspects.   

- [ ] [Architecture and design](#architecture-and-design)  
- [ ] [Code Readability](#code-readability)  
- [ ] [Security](#security)
- [ ] [Error handling](#error-handling)
- [ ] [Logging](#logging)
- [ ] [Input validation](#input-validation)
- [ ] [Transactions](#transactions)
- [ ] [Performance under load](#performance-under-load)
- [ ] [Compliance with the requirements](#compliance-with-the-requirements)
- [ ] [Back-Compatibility](#back-compatibility)
- [ ] [Third-party libraries](#third-party-libraries)
- [ ] [Documentation](#documentation)
- [ ] [Automated tests](#automated-tests)

Note: all these aspects are equally important, order in this list does not matter.
See also [Useful Resources](#useful-resources) at the bottom of this page. 


## Architecture and Design

- [ ] **Single Responsibility Principle**:  A class or method should have one-and-only-one responsibility. If we have to use “and” to finish describing what a method is capable of doing, it might be at the wrong level of abstraction.

- [ ] **Loose coupling**: Aim on modules of an application to be independent as much as possible. Components should not have any knowledge or very limited knowledge about other components or parts of the whole system.

- [ ] **Code duplication**:  Go by the “three strikes” rule. If code is copied once, it’s usually okay although not ideal. If it’s copied again, it should be refactored so that the common functionality is split out. (Can be automated)

- [ ] **Magical strings or numbers**: Are string or numbers used in code moved to constants with meaningful names?

- [ ] **Potential bugs**: Are there off-by-one errors? Will the loops terminate in the way we expect? Will they terminate at all?

- [ ] **Error handling**: Are errors handled gracefully and explicitly where necessary? Have custom errors been added? If so, are they useful?

- [ ] **Efficiency**: If there’s an algorithm in the code, is it using an efficient implementation? 

- [ ] **Potential memory leaks**:  Are all event listeners removed when not used? Is data cleaned up if no longer used? 

## Code readability

**1. Methods and functions**

- [ ] Does the method name have the form “expressive verb + object”? 
```
getName, sendEmail, printMessage, saveUsers
```

- [ ] Have you chosen verbs and nouns that you prefer to use on the project? Stick to them. Examples:
```
add/remove, increment/decrement, open/close, begin/end, insert/delete, show/hide, create/destroy
```

- [ ] Is it clear from method name what type of data it returns? For example,
```
isValid - @returns boolean
getUser - @returns User
sendMessage - @returns nothing or true/false
```

- [ ] Are there too many parameters for the method? If there are more than 4 parameters - it is probably a sign that it could be grouped in a different way
```
sendMessage (to, subject, body, ...)
sendMessage ({ to: ..., subject: ..., body: … })
```

- [ ] Are parameters ordered in the same way as in other similar methods?  Example of inconsistent order:
```
logError(errorType, data) 
logWarning(data, errorType) - WRONG, inconsistent order
```

- [ ] Does the method name describe all the actions it performs?

- [ ] Is method too big? The rule of thumb is that a function should be less than 20 or so lines. If I see a method above 50, I feel it’s best that it be cut into smaller pieces. (Can be automated)

- [ ] Is the method doing only one thing, but well? 
(single responsibility principle)

- [ ] Is there a logic, which should be extracted as separate methods?

- [ ] Does the method change the environment, global variables, its input data? Is it obvious that method does this?

- [ ] Does the method return the correct value in all possible cases?

**2. Variables**

- [ ] Does each variable have one and only one goal?

- [ ] Are the variables named according to the convention (camelCase, kebab-case, etc)

- [ ] Does the variable name describe the represented entity fully and accurately?

- [ ] Does the name of the variable have a length sufficient to ensure that it does not need to be puzzled over it?

- [ ] Is it possible to understand by the name of variable, which kind of data it keeps?

- [ ] Are there any magic numbers or strings?

**3. Data Type Conversion**

- [ ] Are type conversions obvious? 

- [ ] If variables of two different types are used in the same expression, will it be calculated the way you expect it? 

- [ ] Is there a comparison of variables of different types?

**4. Classes** 

- [ ] Does the class have a central purpose?

- [ ] Is the class well named, and does its name describe its central purpose?

- [ ] Is the class independent of other classes? Is it loosely coupled?

- [ ] Does the class collaborate with other classes only when it’s absolutely necessary?
 
- [ ] Does the class hide its implementation details from other classes as much as it’s necessary?

**5. Logical Checks**

- [ ] Are there complex logical checks? Can you use variables or functions to improve readability?

**6. Data Structures**

- [ ] Do you use data structures instead of variable sets to organize related data? 
- [ ] Did you consider class creation as an alternative to using structure?

- [ ] Do you use Enum types where appropriate?

**7. Conditional expressions**

- [ ] Are complex checks converted to calls of logical functions?
 
- [ ] Are the most likely cases checked first? Are all options considered?

- [ ] Does the method contain more than 10 conditional expressions? Is there a good reason for not redesigning it?

**8. Recursion**

- [ ] Does the recursive method contain code to stop recursion? 

- [ ] Does the method use a security counter to ensure that execution will be completed?

- [ ] Does the recursion depth match the constraints imposed by the size of the program stack?

**9. Copy / Paste**

- [ ] Is there a copy/pasted code with minimal changes? Consider refactoring.


## Security

- [ ] It should not be possible to execute user-provided code on the server: 
  sanitize the input in a very srtrict way.

- [ ] It should not be possible to do SQL injection

- [ ] API should not return secret or personal user data if it’s not needed by frontend   
 (email, password, credit card number, phone number, address)

- [ ] It should not be possible to use “brute-force attack”. Add some counter to protect from that.  

- [ ] Don’t send sensitive information to logs (passwords, API keys, database credentials, etc.)

- [ ] Don’t send sensitive information in error messages from server!

- [ ] Use CSRF tokens to protect from Cross-Site request forgery attacks

## Error Handling


- [ ] Is the error handling technique defined in the project? You should define format of errors (messages, status codes, ..) and where do you send errors (console, file, AWS S3, etc.)

- [ ] Is the error handling code centralized in a single module that can be replaced if necessary?

- [ ] Aren’t errors ignored somewhere? (empty “catch” block means that error is ignored, and it will be hard to debug that) 

- [ ] Don’t you return too much information to the end user when error happens? (system paths, parameters of database connection

## Logging 

- [ ] Is there enough logging? It’ll help to debug problems on production.

- [ ] Isn’t there too much logging?

- [ ] Are you sure that you don’t send sensitive information to logs (passwords, API keys, database credentials, etc.)

- [ ] Is the logging library centralized in a single module that can be replaced if necessary?

- [ ] Are messages divided by severity level (info, warning, error)?

- [ ] Can you disable some levels of logging (info, debug)?

- [ ] Can you easily add new destinations where logs are sent (console, file, AWS S3, etc.)? 

## Input validation


- [ ] Do you have protection from incorrect input?

- [ ] Did you define a format of validation errors which you return?
  (HTTP status code and error response format)

- [ ] File upload: is there a protection against too large files? 

- [ ] File upload: do you check that file type is the one you expect?


## Transactions

- [ ] In actions which require multiple steps (signup, checkout, etc.) is it possibe to restart if user got an error on some step?

## Performance under load

- [ ] Can you tell the complexity of the algorithm? O (n), O (n2), O (n * n * n)?

- [ ] How will the program behave if there is unexpectedly lot of data arrives?

- [ ] How will the program behave if there is unexpectedly lot of requests arrive?

**Race conditions**

- [ ] Can race conditions arise?

- [ ] Can user accidentally perform the same action twice?


## Compliance with the requirements

- [ ] Does the code do exactly what is indicated in the requirements?

- [ ] Didn’t you miss small but important details?

- [ ] Haven’t you done your own assumptions instead of asking the customer or PM?

## Back compatibility

- [ ] Do the changes break back-compatibility?

- [ ] Are the APIs and interfaces extensible? 

## Third-party libraries

- [ ] Is this library really needed?

- [ ] Is the library distributed under an open source license?

- [ ] Are there any known vulnerabilities in the library?

- [ ] Are maintainers and community active? 

- [ ] Is it easy to contact them in case of issues?

- [ ] Does the library have any known vulnerabilities?

- [ ] Can you use only some functions from that library to reduce build size?

## Documentation

- [ ] Are there complex pieces in the code and do comments describe them?

- [ ] Are data structures and units of measurement explained (at least with Flow annotations)?

- [ ] Is any unusual behaviour or edge-case handling described?

- [ ] Is there unfinished or temporary code? If yes, it should be deleted or marked 'TODO'

- [ ] Commented code: should be removed

## Automated tests

- [ ] Does each requirement have its own test case?

- [ ] Does each element you designed (module, class, function) have its own test case?

- [ ] Has each line of code been tested with at least one test case? 

- [ ] Have all simple boundaries been tested: maximum, minimum, and off-byone boundaries?

- [ ] Do test cases check for the wrong input data—for example, a negative number of employees in a payroll program?

- [ ] Is compatibility with old data tested? 

- [ ] Is the minimum/maximum configuration tested?

- [ ] How fragile are tests? Do tests depend on each other?

- [ ] What's the time tests run? Can it be sped up



### Useful Resources

- [Code Review - The Ultimate Guide](https://www.airpair.com/code-review/posts/code-review-the-ultimate-guide)
- [Code Review Checklist – To Perform Effective Code Reviews](https://www.evoketechnologies.com/blog/code-review-checklist-perform-effective-code-reviews/)  
- [Gerrit - Code review checklist](https://www.mediawiki.org/wiki/Gerrit/Code_review)  
- [Java code review checklist](https://dzone.com/articles/java-code-review-checklist)  
- [A guide for reviewing code](https://github.com/thoughtbot/guides/tree/master/code-review)  
- [One More Code Review Checklist](https://dev.to/tomerbendavid/code-review-checklist-1k81)  
