------------------------------------------------------------------------------------------
Collections
------------------------------------------------------------------------------------------

1. Draw Collections Framework Class Diagram

    interface Iterable
    interface Collection extends Iterable
    interfaces List, Queue, Set extend Collection
    interface SortedSet extends Set
    interface Map
    interface SortedMap extends Map
    classes ArrayList, Vector, Stack, LinkedList implement List
    classes HashSet, LinkedHashSet implement Set
    class TreeSet implements SortedSet
    classes HashMap, WeakHashMap, LinkedHashMap implement Map
    class TreeMap implements SortedMap
    utility classes Collections and Arrays

2. What is HashMap and Map?

    Map is an interface. Contains methods to manipulate Key-Value based collections. The main methods of Map interface are put(K,V), get(K), Collection<V> values(), Set<K> keySet(), containsKey(), containsValue()
    HashMap is one of implementations of the Map interface based on hashcodes of objects used as keys.

3. Difference between HashMap and HashTable? Can we make hashmap synchronized?

    Both implement Map interface. HashTable is synchronized. It is recommended to use HashMap wherever possible. HashTable doesn't allow null keys and values. HashMap allows one null key and any number of null values.
    We can make it synchronized
        Map m = Collections.synchronizedMap(new HashMap());

4. Difference between Vector and ArrayList?

    Both implement List interface. ArrayList is not synchronized.

5. What is an Iterator?

    It is an interface that contains three methods:  next(), boolean hasNext(), void remove()
    It allows to iterate over the collection
    If the class implements iterator then it can be used in foreach loop

6. List vs Set vs Map. Purposes and definitions.

    All three are interfaces.

    List -- storing values in specified order. Provides methods to get the element by its position get(i), finding element, ListIterator. Known implementations: ArrayList, Vector, LinkedList. List should be used when the order in which the elements are stored matters.

    Set -- storing only different objects and at most one null element. Known implementations: TreeSet (iterate over the elements in order defined by Comparator, or if the elements implement comparable; provides log(n) performance for basic operations), HashSet -- stores values in buckets defined by their hashcodes. Each bucket is a singly linked list. Provides constant time performance for basic operations. LinkedHashSet

    Map -- for storing key-value pairs. Map cannot contain duplicate keys. Provides three collection views: set of keys, collection of values, set of key-value mappings. Know implementations HashMap, EnumMap, TreeMap, LinkedHashMap, WeakHashMap.

7. Pros and cons of ArrayList and LinkedList

    ArrayList -- fast random access.
    LinkedList -- slow random access. Implements Queue interface. Fast deletion of the element.
    If lots of random reads is anticipated use ArrayList.
    If lots of iterations over the whole list and lots of add/delete -- use LinkedList.

8. TreeSet vs LinkedHashSet

    LinkedHashSet is backed by LinkedHashMap. LinkedHashMap is backed by doubly linked list to enforce ordering on the elements contained in the Map.
    If the ordering of the elements in the Set matters to you but you don't want to use a comparator you may use LinkedHashSet since it will enforce ordering in which the elements were added to the set. Otherwise use TreeSet

9. What are relationships between equals and hash codes?

    See in Java Basics.

10. What are the advantages of ArrayList over arrays ?

    1. ArrayList comes with a number of utility methods (e.g. contains, remove, addAll)
    2. Type safety
    3. Dynamic sizing
    On the other hand arrays are a little bit faster and take less memory (packing). Arrays are also able to contain values of primitive types while ArrayList can only contain Objects.

11. Principle of storing data in a hashtable

    HashSet. add(element) -> element.hashCode() -> mod bucketsCount -> store
    HashMap. add(key, value) -> key.hashCode() -> mod bucketsCount -> store(value)

12. Differences between Hashtable, ConcurrentHashMap and Collections.synchronizedMap()

    ConcurrentHashMap allows concurrent modification of the Map from several threads without the need to block them. Collections.synchronizedMap(map) creates a blocking Map which will degrade performance, albeit ensure consistency (if used properly).
    Use the second option if you need to ensure data consistency, and each thread needs to have an up-to-date view of the map. Use the first if performance is critical, and each thread only inserts data to the map, with reads happening less frequently.

13. How are hash codes computed?

    if hashCode() method is defined then it is called to calculate the hashcode
    if its not defined the default implementation in Object class does the following:

        public int hashCode() {
            return VMMemoryManager.getIdentityHashCode(this);
        }

14. Is it possible that hash code is not unique?

    It's totally possible. Actually a totally valid hashCode() function could look like this

    int hashCode(){ return 57; }

15. Can we put two elements with equal hash code to one hash map?

    Yes we can. The hashcode of objects doesn't matter. Only the hashcode of keys. But even if you want to put keys with the same hashcode it will be ok since it just means that key-value pairs will be put into the same bucket

16. Iterator and modification of a List. ConcurentModificationException.

    The iterators returned by this class's iterator method are fail-fast: if the set is modified at any time after the iterator is created, in any way except through the iterator's own remove method, the iterator will throw a ConcurrentModificationException. Thus, in the face of concurrent modification, the iterator fails quickly and cleanly, rather than risking arbitrary, non-deterministic behavior at an undetermined time in the future.

    Note that the fail-fast behavior of an iterator cannot be guaranteed as it is, generally speaking, impossible to make any hard guarantees in the presence of unsynchronized concurrent modification. Fail-fast iterators throw ConcurrentModificationException on a best-effort basis. Therefore, it would be wrong to write a program that depended on this exception for its correctness: the fail-fast behavior of iterators should be used only to detect bugs.

17. What is the significance of ListIterator? What is the difference b/w Iterator and ListIterator?

    ListIterator allows to perform iteration both ways (first-->last and last-->first)
    From JavaDoc: ListIterator is an iterator for lists that allows the programmer to traverse the list in either direction, modify the list during iteration, and obtain the iterator's current position in the list

18. What is the Collections API?

    See above

19. How can we access elements of a collection?

    Depends on the collection type. In general: using an iterator, getting by index, getting by key, using iterator on a view

20. What’s the difference between a queue and a stack?

    In Java Queue is an interface and has a number of implementations. Stack is a class derieved from Vector class.
    The Queue interface contains methods to enqueue and dequeue elements: offer(), poll(). Also contains methods to see the first element in the queue: peek(). The queue ordering is not neccessairly FIFO. It can be based on the comparator (PriorityQueue)
    The Stack contains methods to add and remove elements to/from a stack. If used correctly enforces LIFO policy. Contains methods push(), pop(), peek(). The problem with Java stack implementation is that it contains all the methods from Vector class which can lead to violation of LIFO policy.

21. What is the Properties class?

    Property class is designed to hold properties (i.e. pairs <String, String> -- property name (key) and property value). The two main methods of Properties class is getProperty(String name) and setProperty(String name, String value). Properties class extends HashTable

22. Which implementation of the List interface provides for the fastest insertion of a new element into the middle of the list?

    LinkedList. It only requires 4 assignments.
    Before: ... <--> A <--> B <--> ...
    After:  ... <--> A <--> C <--> B <--> ... . A changes one reference, B changes one reference, C assigns two references.

23. How can we use hashset in collection interface?

    I'm not sure I understand this question fully. HashSet implements Set interface which in turn extends the Collection interface. Thus HashSet is a valid class to be used everywhere where Collection interface is required.

25. Can you limit the initial capacity of vector in java?

    I'm not sure if limit is correct word to be used in this question. One can just set the initial capacity of the Vector. Well you could say that you are putting lower limit on the size of the Vector.

26. What method should the key class of Hashmap override?

    equals() and hashCode().

27. What is the difference between Enumeration and Iterator?

    Enumeration is an interface similar to Iterator. But it doesn't have remove() method. Enumeration acts as Read-only interface, because it has the methods only to traverse and fetch the objects, where as using Iterator we can manipulate the objects also like adding and removing the objects

28. Collections class and Arrays class

    The Collections class contains a number of factory methods for creating unmodifiable views or synchronized wrappers of collections, as well as utility methods such as sort, shuffle, reverse

    The Arrays class contains a factory method that take an array as input and return a List based on this array (Arrays.asList()). Also contains methods to sort, search, fill and print arrays
