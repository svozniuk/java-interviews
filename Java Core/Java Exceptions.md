------------------------------------------------------------------------------------------
Exceptions
------------------------------------------------------------------------------------------

1. What are Checked and UnChecked Exception?

    Checked -- exceptions that the program has to handle with and to be able to succesfully recover from.
    Unchecked (Runtime) -- usually signal of a programmatic error. Potentially can happen anywhere
    Error -- something really bad happened to the JVM or system. Usually cannot be recovered from.

2. What are runtime exceptions?

    See above.

3. What is the difference between error and an exception?

    See above.

4. How to create custom exceptions?

    Extend from Exception class or from RuntimeException class.

5. If I want an object of my class to be thrown as an exception object, what should I do?

    Extend from Exception class (or any of its subclasses)

6. If my class already extends from some other class what should I do if I want an instance of my class to be thrown as an exception object?

    Nothing. There is no Exception interface.

7. How does an exception permeate through the code?

    It's either caught or it is going to be passed to the next level in call stack.
    If it's caught it can be rethrown as another Exception which has to be processed in a higher level.

8. What are the different ways to handle exceptions?

    Declare that the methods throws an exception or catch it.

9. What is the basic difference between the 2 approaches to exception handling...1> try catch block and 2> specifying the candidate exceptions in the throws clause?

    The main difference is in who is responsibile for handling the exception.

10. When should you use which approach?

    As I understand one should handle the exception if he/she has all the knowledge and resources to do so. And the exception has to be handled as soon as this knowledge is aquired. However if you are writing a library you may consider declaring the exceptions that are thrown and force the user of the library to decide how to react to a specific situation. The downside of this approach is that declared exceptions become the part of the API which has to be frozen after being published.

11. Is it necessary that each try block must be followed by a catch block?

    No. It can be followed by either catch block or finally block or both.

12. If I write return at the end of the try block, will the finally block still execute?

    Yes it will.

13. If I write System.exit (0); at the end of the try block, will the finally block still execute?

    No it won't. It also won't execute if the thread where the exception is thrown is terminated.

14. How does a try statement determine which catch clause should be used to handle an exception?

    The first that has a matching exception type declared.

15. What is the catch or declare rule for method declarations? 

    blah-blah-blah. If you are using a checked exception you either have to catch it or declare that it could be thrown by a method.

16. Pros and cons of using exceptions vs. returning "error value"

    Exceptions provide clearer understanding and better code
    Exceptions can be grouped together to be handled
    Exceptions can be propagated up in the call stack to be handled where they are supposed to be handled.
    Exception handling could be slow.
    In some sense Exceptions are unignorable error codes.

    For example, exceptions are intrusive (eg if they're not handled the program dies).  If you have an error that can be safely ignored, being forced to catch and handle can be painful. However, they're definitely appropriate if the result of quietly ignoring an error is something that will bite you later.

17. Cases when the finally block isn't executed

    System.exit() in try or catch block.
    Error in one of the blocks (e.g. OutOfMemoryError)
    Thread is terminated, while the whole application still continues.

18. What is exception handling mechanism

    Blah-blah-blah.

19. Bundled exceptions

    IllegalArgumentException
    ConcurrentModificationException
    IllegalStateException
    UnsupportedOperationException
    ArithmeticException
    NullPointerException
    ArrayIndexOutOfBoundsException
    IOException

20. How to avoid catch block?

    Declare that method throws the exception. You can still use try/finally.
