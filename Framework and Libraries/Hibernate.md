-----------------     Hibernate      -----------------

Benefits and risks of using Hibernate
  //todo

What is a SessionFactory? Is it a thread-safe object?
    The main contract here is the creation of Session instances. Usually an
  application has a single SessionFactory instance and threads servicing client
  requests obtain Session instances from this factory.
    The internal state of a SessionFactory is immutable. Once it is created this
  internal state is set. This internal state includes all of the metadata about
  Object/Relational Mapping.
    Implementors must be threadsafe.

What is the difference between merge and update
  saveOrUpdate:
    Calls either save or update depending on some checks. E.g. if no identifier
    exists, save is called. Otherwise update is called.
  save:
    Persists an entity. Will assign an identifier if one doesn't exist. If one
    does, it's essentially doing an update. Returns the generated ID of the
    entity.
  update:
    Update the persistent instance with the identifier of the given detached
    instance. If there is a persistent instance with the same identifier,
    an exception is thrown. This operation cascades to associated instances
    if the association is mapped with cascade="save-update".
  merge:
    Copy the state of the given object onto the persistent object with the same
    identifier. If there is no persistent instance currently associated with
    the session, it will be loaded. Return the persistent instance. If the
    given instance is unsaved, save a copy of and return it as a newly
    persistent instance. The given instance does not become associated with
    the session. This operation cascades to associated instances if the
    association is mapped with cascade="merge".
    In these cases you need to use merge for updates and persist for saving.
  persist:
    As mentioned above, this is used on transient objects. It does not return
    the generated ID.

What is Hibernate Query Language (HQL)?
  Hibernate uses a powerful query language (HQL) that is similar in appearance
  to SQL. Compared with SQL, however, HQL is fully object-oriented and
  understands notions like inheritance, polymorphism and association.

Lazy loading
  Proxy pattern.
    Say you have a parent and that parent has a collection of children.
  Hibernate now can "lazy-load" the children, which means that it does not
  actually load all the children when loading the parent. Instead, it loads them
  when requested to do so. You can either request this explicitly or, and this
  is far more common, hibernate will load them automatically when you try to
  access a child.
    Lazy-loading can help improve the performance significantly since often you
  won't need the children and so they will not be loaded.
    Also beware of the n+1-problem. Hibernate will not actually load all
  children when you access the collection. Instead, it will load each child
  individually. When iterating over the collection, this causes a query for
  every child. In order to avoid this, you can trick hibernate into loading
  all children simultaneously, e.g. by calling parent.getChildren().size().

What do you mean by Named � SQL query and how to invoke it?
  Named SQL queries may be defined in the mapping document and called in exactly
  the same way as a named HQL query. In this case, we do not need to call
  addEntity().
  <sql-query name="persons">
      <return alias="person" class="eg.Person"/>
      SELECT person.NAME AS {person.name},
             person.AGE AS {person.age},
             person.SEX AS {person.sex}
      FROM PERSON person
      WHERE person.NAME LIKE :namePattern
  </sql-query>
  ..
  List people = session.getNamedQuery("persons")
      .setString("namePattern", namePattern)
      .setMaxResults(50)
      .list();

What is the difference between sorted and ordered collection in hibernate?
  sorted collection:
    A sorted collection is sorting a collection by utilizing the sorting features
    provided by the Java collections framework. The sorting occurs in the memory
    of JVM which running Hibernate, after the data being read from database using
    java comparator. If your collection is not large, it will be more efficient
    way to sort it.
  order collection :
    Order collection is sorting a collection by specifying the order-by clause
    for sorting this collection when retrieval. If your collection is very large,
    it will be more efficient way to sort it.

Difference between get() and load()?
  If load() can�t find the object in the cache or database, an exception is
  thrown. The load() method never returns null. The get() method returns
  null if the object can�t be found.
  The load() method may return a proxy instead of a real persistent instance.
  A proxy is a placeholder that triggers the loading of the real object when it�s
  accessed for the first time; we discuss proxies later in this section. On the
  other hand, get() never returns a proxy.

What are the types of Hibernate instance states ?
  transient
    The instance is not associated with any persistence context. It has no
    persistent identity or primary key value.
  persistent
    The instance is currently associated with a persistence context. It has a
    persistent identity (primary key value) and can have a corresponding row in
    the database. For a particular persistence context, Hibernate guarantees
    that persistent identity is equivalent to Java identity in relation to the
    in-memory location of the object.
  detached
    The instance was once associated with a persistence context, but that
    context was closed, or the instance was serialized to another process.
    It has a persistent identity and can have a corresponding row in the
    database. For detached instances, Hibernate does not guarantee the
    relationship between persistent identity and Java identity.

Hierarchy-to-tables mapping strategies //todo check with reference doc (table per class hierarchy | table per subclass | table per concrete class | implicit polymorphism )
    Table per concrete class with implicit polymorphism�Use no explicit
  inheritance mapping, and default runtime polymorphic behavior.
  You can use exactly one table for each (nonabstract) class.
  All properties of a class, including inherited properties, can be mapped to
  columns of this table,
    Table per concrete class�Discard polymorphism and inheritance relationships
  completely from the SQL schema. Table per concrete class with unions.
  If your superclass is concrete, then an additional table is needed to hold
  instances of that class.
    Table per class hierarchy�Enable polymorphism by denormalizing the SQL
  schema, and utilize a type discriminator column that holds type information.
  An entire class hierarchy can be mapped to a single table.
  This table includes columns for all properties of all classes in the hierarchy.
  The concrete subclass represented by a particular row is identified by the
  value of a type discriminator column.
  There is one major problem: Columns for properties declared by subclasses
  must be declared to be nullable.
  Another important issue is normalization. We�ve created functional
  dependencies between nonkey columns, violating the third normal form.
    Table per subclass�Represent is a (inheritance) relationships as has a
  (foreign key) relationships.
  The fourth option is to represent inheritance relationships as relational
  foreign key associations. Every class/subclass that declares persistent
  properties�including abstract classes and even interfaces�has its own table.
  Unlike the table per concrete class strategy we mapped first, the table here
  contains columns only for each noninherited property (each property declared
  by the subclass itself) along with a primary key that is also a foreign key of
  the superclass table.
  The primary advantage of this strategy is that the SQL schema is normalized.
  Schema evolution and integrity constraint definition are straightforward.
  A polymorphic association to a particular subclass may be represented as a
  foreign key referencing the table of that particular subclass.
  As you can see, this mapping strategy is more difficult to implement by hand�
  even ad-hoc reporting is more complex. This is an important consideration if
  you plan to mix Hibernate code with handwritten SQL.
  Furthermore, even though this mapping strategy is deceptively simple, our
  experience is that performance can be unacceptable for complex class
  hierarchies. Queries always require either a join across many tables or many
  sequential reads.

What are the Collection types in Hibernate ?
  A collection is defined as a one-to-many reference. The simplest collection
  type in Hibernate is <bag>.This collection is a list of unordered objects and
  can contain duplicates. This is similar to java.util.List. The content of a
  collection is required for a SQL query. This won�t work until the code is
  actually accessed. This process has a benefit that allows the developer to
  separate the database access logic from that of the object traversal logic.
  This is called lazy collection.
  Filtering logic can be applied with lazy collection. This can alter the SQL
  which invokes when the actual collection is invoked.
  ArrayType,
    Constructor: ArrayType(String role, String propertyRef, Class elementClass, boolean isEmbeddedInXML)
  BagType,
    Constructor: BagType(String role, String propertyRef, boolean isEmbeddedInXML)
  CustomCollectionType, A custom type for mapping user-written classes that implement PersistentCollection
    Constructor: CustomCollectionType(Class userTypeClass, String role, String foreignKeyPropertyName, boolean isEmbeddedInXML)
  IdentifierBagType,
    Constructor: IdentifierBagType(String role, String propertyRef, boolean isEmbeddedInXML)
  ListType,
    Constructor: ListType(String role, String propertyRef, boolean isEmbeddedInXML)
  MapType,
    Constructor: MapType(String role, String propertyRef, boolean isEmbeddedInXML)
  SetType
    Constructor: SetType(String role, String propertyRef, boolean isEmbeddedInXML)
