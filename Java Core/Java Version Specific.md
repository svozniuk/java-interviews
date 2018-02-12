------------------------------------------------------------------------------------------
Java 5 specifics
------------------------------------------------------------------------------------------

1. What is a parameterized or generic type?

    Definition from JLS: A class or interface whose declaration has one or more type parameters is a generic class or interface. Each generic type defines a set of parameterized types, which consist of the class or interface name followed by an angle-bracketed list of actual type parameters corresponding to the generic type’s formal type parameters.

    Generic classes and interfaces are collectively known as generic types.

    With generics, you tell the compiler what types of objects are permitted in each generic type. The compiler inserts casts for you automatically and tells you at compile time if you try to insert an object of the wrong type. This results in programs that are both safer and clearer, but these benefits come with complications

2. Can we use parameterized types in exception handling?

    Not sure I understand this question. You cannot write something like public MyException<T> extends Exception. The main problem why its not possible is that type information is not present at the runtime (erasure). Same goes for parametrized catch clause.
    However one can use type parameter in throws clause. More info can be found here
    http://www.angelikalanger.com/GenericsFAQ/FAQSections/TechnicalDetails.html#Topic5

3. What is a wildcard parameterized type?

    It is a generic type that is parametrized with an unbounded ( <?> ) or bounded ( <? extends T> ot <? super T> ) wildcard type. A wildcard type basically tells us that the type parameter can be any type.

4. What is Autoboxing/unboxing?

    Autoboxing is automatic casting of variables of primitive types to corresponding wrapper class. Unboxing is the inverted cast.
    Useful to reduce the amount of code to be written. Pitfalls: can be unintentionally slow if used in a loop. Also care has to be taken with cached values. (e.g. a pool of Integers from -128 to +127)

5. Problems Enum type solves (comparing to "public static int" enum pattern)

    1. Creates a named type to be used instead of plain int numbers. (type safety guarantees)
    2. Ensures that each instance of Enum type is a singleton
    3. Allows custom constant specific behavior
    4. Constants can be easily translated into printable strings.
    5. Enables to associate additional data with enum constants

6. What are Annotations and which predefined by the language specification does one know (@Deprecated, @Override, @SuppressWarnings)

    From http://docs.oracle.com/javase/tutorial/java/javaOO/annotations.html
    Annotations provide data about a program that is not part of the program itself. They have no direct effect on the operation of the code they annotate. Annotations can be applied to a program's declarations of classes, fields, methods, and other program elements.

    @Deprecated -- to inform that the method is deprecated
    @Override -- to inform that this method has to override a method in a superclass. Helps to find errors.
    @SuppressWarnings -- to suppress warnings. E.g. warnings generated when doing potentially unsafe cast
    @SafeVarargs -- the implementation of the method will not improperly handle the varargs formal parameter with a type parameter

7. Annotation retentions policies and why they are in place?

    The retention policies specify how long annotations are to retained.

    CLASS: Annotations are to be recorded in the class file by the compiler but need not be retained by the VM at run time. Useful for bytecode postprocessing. Cannot be inspected at run-time with reflection getAnnotations

    RUNTIME: Annotations are to be recorded in the class file by the compiler and retained by the VM at run time, so they may be read reflectively. (@Deprecated)

    SOURCE:  Annotations are to be discarded by the compiler. (Example: @Override, @SuppressWarnings)

8. Suppose you would like to reuse a class in different contexts would you use annotations or external configuration? (i.e. annotation introduce dependencies).

    Good discussion of this question can be found here
    http://javidjamae.com/2006/11/19/annotations-vs-xml/


------------------------------------------------------------------------------------------
Java 7 specifics
------------------------------------------------------------------------------------------
1. Try with resource

    The try-with-resources statement is a try statement that declares one or more resources. A resource is an object that must be closed after the program is finished with it. The try-with-resources statement ensures that each resource is closed at the end of the statement. Any object that implements java.lang.AutoCloseable, which includes all objects which implement java.io.Closeable, can be used as a resource.

    Example

    static String readFirstLineFromFile(String path) throws IOException {
        try (BufferedReader br =
                       new BufferedReader(new FileReader(path))) {
            return br.readLine();
        }
    }

2. Diamond operator

    In Java 7 writing the type parameters twice is no longer needed.
    Example:

        Instead of writing HashMap<String, Integer> hm = new HashMap<String,Integer>();
        one can just write HashMap<String, Integer> hm = new HashMap<>();
        The latter is now preferable

3. Better exception catching

    In Java 7 the same processing can be performed for a whole list of exceptions:

    try {
        ...
    } catch (ExceptionType1 | ExceptionType2 | ExceptionType3 ex){
        // do some processing
    }

    Note that these exceptions cannot belong to the same hierarchy (none is subclass or superclass of the other)

4. Strings in switch statement

    String dayOfWeek = "Mon";
    switch (dayOfWeek.toLowerCase()) {
        case "mon" :
            //do something
            break;
        case "tue" :
            //do something
            break;
        ...
        default :
            //do something
            break;
    }

5. Numeric literals with underscores

    From http://docs.oracle.com/javase/7/docs/technotes/guides/language/underscores-literals.html
    In Java SE 7 and later, any number of underscore characters (_) can appear anywhere between digits in a numerical literal. This feature enables you, for example, to separate groups of digits in numeric literals, which can improve the readability of your code

    long creditCardNumber = 1234_5678_9012_3456L;
    long socialSecurityNumber = 999_99_9999L;
    float pi =     3.14_15F;


6. New I/O.

    Lots of stuff here. More info cat be found at
    http://docs.oracle.com/javase/tutorial/essential/io/fileio.html
    http://www.drdobbs.com/jvm/java-se-7-new-file-io/231600403

7. Fork/Join framework

    The fork / join framework is an implementation of the ExecutorService interface that helps you take advantage of multiple processors. It is designed for work that can be broken into smaller pieces recursively.

    Example can be found here
    http://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html
