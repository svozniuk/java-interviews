------------------------------------------------------------------------------------------
Generics
------------------------------------------------------------------------------------------

Very good collection of Q&A about Java Generics can be found 
http://www.angelikalanger.com/GenericsFAQ/JavaGenericsFAQ.html


Questions
-------------------------------

1. What is Generics in Java ? What are advantages of using Generics?

    They allow "a type or method to operate on objects of various types while providing compile-time type safety." A common use of this feature is when using a Java Collection that can hold objects of any type, to specify the specific type of object stored in it.

    A class is generic if it declares one or more type variables.

2. How Generics works in Java ? What is type erasure ?

    The type safety is ensured during the compile time. In run time the information about type arguments is erased. The type arguments are replaced by their narrowest superclass. For unbounded parameters -- Object. For bounded -- the declared bound.

3. What is Bounded and Unbounded wildcards in Generics ?

    The wildcard type is signified by "?" in Java. Classes parametrized by wildcard are usually read as "class of some type", e.g. List<?> is read List of some type.

    A wildcard parameterized type is an instantiation of a generic type where at least one type argument is a wildcard.  Examples of wildcard parameterized types are Collection<?>, List<? extends Number>, Comparator<? super String> and Pair<String,?>.A wildcard parameterized type denotes a family of types comprising concrete instantiations of a generic type.  The kind of the wildcard being used determines which concrete parameterized types belong to the family.  For instance, the wildcard parameterized type Collection<?> denotes the family of all instantiations of the Collection interface regardless of the type argument.  The wildcard parameterized type List<? extends Number> denotes the family of all list types where the element type is a subtype of  Number.  The wildcard parameterized type Comparator<? super String> is the family of all instantiations of the Comparator interface for type argument types that are supertypes of String.

    A wildcard parameterized type is not a concrete type that could appear in a new expression.  A wildcard parameterized type is similar to an interface type in the sense that reference variables of a wildcard parameterized type can be declared, but no objects of the wildcard parameterized type can be created.  The reference variables of a wildcard parameterized type can refer to an object that is of a type that belongs to the family of types that the wildcard parameterized type denotes.

4. What is wildcard prameter

    In generic code, the question mark (?), called the wildcard, represents an unknown type. The wildcard can be used in a variety of situations: as the type of a parameter, field, or local variable; sometimes as a return type (though it is better programming practice to be more specific). The wildcard is never used as a type argument for a generic method invocation, a generic class instance creation, or a supertype.

5. What is difference between List<? extends T>  and  List <? super T> ?

    The first list may contain elements of type T or any of its subtypes.
    The second list may contain elements of type T or any of its supertypes.

    For any type X such that X is a subtype of T or T itself
        List<X> is a subtype of List<? extends T>

    For any type Y such that Y is a supertype of T or T itself
        List<Y> is a subtype of List<? super T>

    List<?> is supertype of any parametrized list.

6. How to write a generic method which accepts generic argument and return Generic Type?

    Generic method in a generic class

        public class SomeClass<T>{
            private T value;

            // T is already declared
            // since it's type parameter of the class
            public T doSomething(){...}

            // U has to be declared since it's not type parameter of the class
            public <U> List<U> createSingleElementList(U arg){
                List<U> result = new ArrayList<U>();
                result.add(arg);
                return result;
            }
        }

    Static generic method

        public static <T> T doSomething(T value){...}

7. How to write parametrized class in Java using Generics ?

    class SomeClass<T> extends SomeSuperClass implements SomeInterface
    class SomeClass<T extends Comparable<? super T>>
    class SomeClass<K,V>

8. Can you pass List<String> to a method which accepts List<Object>

    No. You cannot do this since there List<Object> is not a superclass of List<String>

9. Can we use Generics with Array?

    No. One cannot use generic types with arrays. See below other limitations of generic types.

10. What are limitations of Generics

    http://docs.oracle.com/javase/tutorial/java/generics/restrictions.html

    Cannot Instantiate Generic Types with Primitive Types
    Cannot Create Instances of Type Parameters
    Cannot Declare Static Fields Whose Types are Type Parameters
    Cannot Use Casts or instanceof with Parameterized Types
        but you can use (obj instanceOf SomeClass<?>)
    Cannot Create Arrays of Parameterized Types
    Cannot Create, Catch, or Throw Objects of Parameterized Types
    Cannot Overload a Method Where the Formal Parameter Types of Each Overload Erase to the Same Raw Type

11. How can you suppress unchecked warning in Java ?

    @SuppressWarnings("unchecked")

12. Difference between List<Object> and raw type List in Java?

    List<Object> explicitly states that it contains objects. The type checks are on. Using raw types disable checking even outside of their own declarations. This would happen if we used raw List.

    Another thing that differs significantly is that List<Object> is not covariant with any other parametrized list, while List is.

    Example:

        public static void printList(List l){
            for (Object obj : l) {
                System.out.println(obj);
            }
        }

        public static void printObjectList(List<Object> l){
            for (Object obj : l) {
                System.out.println(obj);
            }
        }

        public static void main(String[] args) {
            List<Object> objList = Arrays.asList((Object)new Integer(5), (Object)"x");
            List<String> stringList = Arrays.asList("A","B","C");
            List<Integer> intList = Arrays.asList(1, 2, 3);

            printList(objList);            // valid
            printList(stringList);        // valid
            printList(intList);            // valid

            printObjList(objList);        // valid
            printObjList(stringList);    // invalid. compiler error
            printObjList(intList);        // invalid. compiler error
        }

13. Difference between List<?> and List<Object> in Java?

    List<Object> -- only list of objects is allowed.
    List<?> -- any parametrized list and the raw List type is allowed

14. Difference between List<String> and raw type List.

    List<String>
        Only accepts String arguments in methods like add(), set()
        Returns String values in get()
        Takes care casting the stored values to correct class
        Performs the type checks

15. Difference between List<?> and raw type List.

    The raw type and the unbounded wildcard parameterized type have a lot in common.  Both act as kind of a supertype of all instantiations of the corresponding generic type.  Both are so-called reifiable types. Reifiable types can be used in instanceof expressions and as the component type of arrays, where non-reifiable types (such as concrete and bounded wildcard parameterized type) are not permitted.
    In other words, the raw type and the unbounded wildcard parameterized type are semantically equivalent.  The only difference is that the compiler applies stricter rules to the unbounded wildcard parameterized type than to the corresponding raw type. Certain operations performed on the raw type yield "unchecked" warnings.  The same operations, when performed on the corresponding  unbounded wildcard parameterized type, are rejected as errors.

16. Is <T> List<? extends T> x() a useful signature?

    From docs.oracle.com: Using a wildcard as a return type should be avoided because it forces programmers using the code to deal with wildcards. In this specific case the returning list could be thought as of read-only. But the client code would still be able to add null, to clear the list or to remove the elements from the list. Trying to add element would result in compiler errors regarding the wildcard capture errors
    In this specific case one cannot be sure about which exact type of list will be returned from the method x(). Is it going to be List<T> or maybe List<S> (where S extends T)?

17. What restrictions are placed on method overloading?

    Two methods with different generics cannot overload each other e.g. this is not allowed (again because of type erasure):

    void print(List<String> strings);
    void print(List<Double> doubles);


18. Examples of a valid generic type that cannot be expressed with the Java type system and will lead to compiler warnings.

    from http://stackoverflow.com/questions/12254897/a-bug-in-java-type-system

    interface IProducer<T extends Comparable<T>> {
        public Map<Integer, T> getResults();
    }

    interface IEvaluator {
        public <T extends Comparable<T>> double evaluate(Map<Integer, T> results,
            Map<Integer, Double> groundTruth);
    }

    List<IProducer<?>> producerImplementations = lookUpProducers();

    // dynamically load evaluators
    List<IEvaluator> evaluatorImplementations = lookUpEvaluators();

    // pick a producer
    IProducer<?> producer = producerImplementations.get(0);

    // pick an evaluator
    IEvaluator evaluator = evaluatorImplementations.get(0);

    // evaluate the result against the ground truth
    Map<Integer, ?> data = producer.getResults(); // this type works
    double score = evaluator.evaluate(data, groundTruth); // but now this call does not

    You cannot express a type for data in Java type system. It is just not expressible enough.
    But one can still make the following call and it will work

    double score = evaluator.evaluate(producer.getResults(), groundTruth); 

19. Examples where the java compiler will/will not infere the generic type? (examples of code where the compiler will infere the wrong type)

    No idea

20. What generic type information is not erased and can be retrieved from the byte code? 

    Example:

    public class TypeReference<T> {

        private final Type type;

        public TypeReference() {
            Type superclass = getClass().getGenericSuperclass();
            if (superclass instanceof Class) {
                throw new RuntimeException("Missing type parameter.");
            }
            this.type = ((ParameterizedType) superclass).getActualTypeArguments()[0];
        }
    }

    You are able to get the information about the type used to parametrize this class via getActualTypeArguments (reflection)

21. What does the Class Enum<E extends Enum<E>> ensure?

    http://stackoverflow.com/questions/211143/java-enum-definition?lq=1

    It means that the type argument for enum has to derive from an enum which itself has the same type argument. How can this happen? By making the type argument the new type itself. So if I've got an enum called StatusCode, it would be equivalent to:

    public class StatusCode extends Enum<StatusCode>

    Now if you check the constraints, we've got Enum<StatusCode> - so E=StatusCode. Let's check: does E extend Enum<StatusCode>? Yes! We're okay.

    You may well be asking yourself what the point of this is :) Well, it means that the API for Enum can refer to itself - for instance, being able to say that Enum<E> implements Comparable<E>. The base class is able to do the comparisons (in the case of enums) but it can make sure that it only compares the right kind of enums with each other.

22. If the compiler erases all type parameters at compile time, why should you use generics?

    While the compiler erases all type parameters it still performs all the checks on generic code at compile time. Additionally generics enable you to implement generic algorithms.

23. Super type tokens

    http://gafter.blogspot.ch/2006/12/super-type-tokens.html


Write yourself
-------------------------------

1. Write a program to implement LRU cache using Generics ?

    DONE

2. Write a generic method to count the number of elements in a collection that have a specific property (for example, odd integers, prime numbers, palindromes).

    DONE

3. Write a generic method to exchange the positions of two different elements in an array.

    DONE

4. Write a generic method to find the maximal element in the range [begin, end) of a list.

    DONE

5. How do you invoke the following method to find the first integer in a list that is relatively prime (gcd(a,b) = 1) to a list of specified integers? 

    public static <T>
        int findFirst(List<T> list, int begin, int end, UnaryPredicate<T> p)


    DONE


Problems
-------------------------------
1. What is the following class converted to after type erasure?

    public class Pair<K, V> {

        public Pair(K key, V value) {
            this.key = key;
            this.value = value;
        }

        public K getKey(); { return key; }
        public V getValue(); { return value; }

        public void setKey(K key)     { this.key = key; }
        public void setValue(V value) { this.value = value; }

        private K key;
        private V value;
    }

    Just replace both K and V with Object

2. What is the following method converted to after type erasure?

    public static <T extends Comparable<T>>
        int findFirstGreaterThan(T[] at, T elem) {
        // ...
    }

    it is converted to the following

    public static Comparable
        int findFirstGreaterThan(Comparable[] at, Comparable elem) {
        // ...
    }

3. Will the following method compile? If not, why?

    public static void print(List<? super Number> list) {
        for (Number n : list)
            System.out.print(n + " ");
        System.out.println();
    }

    No. ? super Number allows List<Object> to be passed as a parameter to this method. The for(Number n: list) has to be changed to for(Object n: list)
    Another change that would make this method compilable -- change parameter type to List<? extends Number>

4. Will the following class compile? If not, why?

    public class Singleton<T> {

        public static T getInstance() {
            if (instance == null)
                instance = new Singleton<T>();

            return instance;
        }

        private static T instance = null;
    }

    No, it won't. The method is static. Should have been declared public static <T> T getInstance(). The field is also static. Type parameter T is not known in static context.

5. Given the following classes:

    class Shape { /* ... */ }
    class Circle extends Shape { /* ... */ }
    class Rectangle extends Shape { /* ... */ }

    class Node<T> { /* ... */ }

    Will the following code compile? If not, why?

    Node<Circle> nc = new Node<>();
    Node<Shape>  ns = nc;

    No. Generic types are not covariant.

6. Consider this class:

    class Node<T> implements Comparable<T> {
        public int compareTo(T obj) { /* ... */ }
        // ...
    }

    Will the following code compile? If not, why?

    Node<String> node = new Node<>();
    Comparable<String> comp = node;

    Yes. Node<String> implements Comparable<String>

7. Will the following class compile? If not, why?

    public final class Algorithm {
        public static T max(T x, T y) {
            return x > y ? x : y;
        }
    }

    Not this class won't compile
        1. If T is the type parameter then the method should have been declared like
            public static <T> T max(T x, T y)
        2. Even if the method was declare correctly the binary operator ">" is not defined for classes
