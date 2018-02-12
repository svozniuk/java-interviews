------------------ SQL (Oracle applied)
What are DML and DDL
  DDL
    Data definition language (DDL) statements define, structurally change, and
    drop schema objects.
    An implicit COMMIT occurs immediately before the database executes a DDL
    statement and a COMMIT or ROLLBACK occurs immediately afterward.
  DML
    Data manipulation language (DML) statements query or manipulate data in
    existing schema objects. Whereas DDL statements enable you to change the
    structure of the database, DML statements enable you to query or change the
    contents. For example, ALTER TABLE changes the structure of a table, whereas
    INSERT adds one or more rows to the table.

Agregate functions with examples
todo

What is a nested subquery?
    A subquery is a SELECT statement nested within another SQL statement. Subqueries
    are useful when you must execute multiple queries to solve a single problem.
    These subqueries can reside in the WHERE clause, the FROM clause, or the SELECT clause.
    Example:
        SELECT first_name, last_name
        FROM employees
        WHERE department_id
        IN (SELECT department_id FROM departments WHERE location_id = 1800);

Types of indices in Oracle
  Oracle Database provides several indexing schemes, which provide complementary
  performance functionality. The indexes can be categorized as follows:
  B-tree indexes
    These indexes are the standard index type. They are excellent for primary
    key and highly-selective indexes. Used as concatenated indexes, B-tree
    indexes can retrieve data sorted by the indexed columns.
    B-tree indexes have the following subtypes:
    � Index-organized tables
      An index-organized table differs from a heap-organized because the data is
      itself the index. See "Overview of Index-Organized Tables" on page 3-20.
    � Reverse key indexes
      In this type of index, the bytes of the index key are reversed, for
      example, 103 is stored as 301. The reversal of bytes spreads out inserts
      into the index over many blocks.
    � Descending indexes
      This type of index stores data on a particular column or columns in
      descending order.
    � B-tree cluster indexes
      This type of index is used to index a table cluster key. Instead of
      pointing to a row, the key points to the block that contains rows related
      to the cluster key.
  Bitmap and bitmap join indexes
    In a bitmap index, an index entry uses a bitmap to point to multiple rows.
    In contrast, a B-tree index entry points to a single row. A bitmap join
    index is a bitmap index for the join of two or more tables.
  Function-based indexes
    This type of index includes columns that are either transformed by a
    function, such as the UPPER function, or included in an expression.
    B-tree or bitmap indexes can be function-based.
  Application domain indexes
    This type of index is created by a user for data in an application-specific
    domain. The physical index need not use a traditional index structure and
    can be stored either in the Oracle database as tables or externally as a
    file.

What is a hierarchical query? How to create it?
    http://docs.oracle.com/cd/B19306_01/server.102/b14200/queries003.htm
    If a table contains hierarchical data, then you can select rows in a hierarchical order using the hierarchical query clause:
    [ START WITH condition ] CONNECT BY [ NOCYCLE ] condition
    Example:
        1)show EXPLAIN PLAN
        -- EXPLAIN PLAN
        delete PLAN_TABLE where statement_id = 'ago01';
        EXPLAIN PLAN SET STATEMENT_ID = 'ago01' FOR
        ---------------------- put query below
        select  /*+ FIRST_ROWS(25) */
                decode(message_type, '1','personal', '2','broadcast') as mess_type,
                eb_messages_data.*
          from eb_messages_data
          where message_id in (select message_id
                                 from eb_disposer_mailboxes
                                where disposer_number like '%3140007')
                and message_type = '2';
        ----------------------- put query above

        select * from PLAN_TABLE where STATEMENT_ID = 'ago01';

        SELECT lpad('.',level-1, '.')||operation||' '|| options||' '||object_type||' '||object_name "Plan",
               cost, bytes, cardinality, access_predicates || filter_predicates as predicates
          FROM PLAN_TABLE
        CONNECT BY prior id = parent_id
                AND prior statement_id = statement_id
          START WITH id = 0
                AND statement_id = 'ago01'
          ORDER BY id;
          2)CONNECT BY Example
            SELECT employee_id, last_name, manager_id
               FROM employees
               CONNECT BY PRIOR employee_id = manager_id;
          3)level example
            SELECT employee_id, last_name, manager_id, LEVEL
               FROM employees
               CONNECT BY PRIOR employee_id = manager_id;
          4)START WITH Examples
            SELECT last_name, employee_id, manager_id, LEVEL
                  FROM employees
                  START WITH employee_id = 100
                  CONNECT BY PRIOR employee_id = manager_id
                  ORDER SIBLINGS BY last_name;

What is query tuning? How to tune a query?
    http://www.dba-oracle.com/art_sql_tune.htm
    http://www.akadia.com/services/ora_query_tuning.html
    http://www.orafaq.com/wiki/Oracle_database_Performance_Tuning_FAQ

Partitioning and methods of partitioning
        Partitioning enables you to decompose very large tables and indexes into
    smaller and more manageable pieces called partitions. Each partition is an
    independent object with its own name and optionally its own storage characteristics.
        From the perspective of an application, only one schema object exists. DML statements require no modification
    to access partitioned tables. Partitioning is useful for many different types of database applications, particularly
    those that manage large volumes of data. Benefits include:
        Increased availability
        Easier administration of schema objects
        Reduced contention for shared resources in OLTP systems
        Enhanced query performance in data warehouses
    Partitioning Strategies:
        Range Partitioning
            CREATE TABLE time_range_sales
            ( prod_id NUMBER(6)
            , cust_id NUMBER
            , time_id DATE
            , channel_id CHAR(1)
            , promo_id NUMBER(6)
            , quantity_sold NUMBER(3)
            , amount_sold NUMBER(10,2)
            )
            PARTITION BY RANGE (time_id)
            (PARTITION SALES_1998 VALUES LESS THAN (TO_DATE('01-JAN-1999','DD-MON-YYYY')),
            PARTITION SALES_1999 VALUES LESS THAN (TO_DATE('01-JAN-2000','DD-MON-YYYY')),
            PARTITION SALES_2000 VALUES LESS THAN (TO_DATE('01-JAN-2001','DD-MON-YYYY')),
            PARTITION SALES_2001 VALUES LESS THAN (MAXVALUE)
            );
        List Partitioning
            CREATE TABLE list_sales
            ( prod_id NUMBER(6)
            , cust_id NUMBER
            , time_id DATE
            , channel_id CHAR(1)
            , promo_id NUMBER(6)
            , quantity_sold NUMBER(3)
            , amount_sold NUMBER(10,2)
            )
            PARTITION BY LIST (channel_id)
            (PARTITION even_channels VALUES (2,4),
            PARTITION odd_channels VALUES (3,9)
            );
        Hash Partitioning
            CREATE TABLE hash_sales
            ( prod_id NUMBER(6)
            , cust_id NUMBER
            , time_id DATE
            , channel_id CHAR(1)
            , promo_id NUMBER(6)
            , quantity_sold NUMBER(3)
            , amount_sold NUMBER(10,2)
            )
            PARTITION BY HASH (prod_id)
            PARTITIONS 2;

------------------ DB
Primary key vs unique key. Differences.

What types of constraints does one know?
  NOT NULL
    Allows or disallows inserts or updates of rows
    containing a null in a specified column.
  Unique key
    Prohibits multiple rows from having the same value in
    the same column or combination of columns but allows
    some values to be null.
  Primary key
    Combines a NOT NULL constraint and a unique constraint. It prohibits
    multiple rows from having the same value in the same column or combination
    of columns and prohibits values from being null
  Foreign key
    Designates a column as the foreign key and establishes a relationship
    between the foreign key and a primary or unique key, called the referenced
    key.
  Check
    Requires a database value to obey a specified condition.
  REF
    Dictates types of data manipulation allowed on values in a REF column and
    how these actions affect dependent values. In an object-relational database,
    a built-in data type called a REF encapsulates a reference to a row object
    of a specified object type. Referential integrity constraints on REF columns
    ensure that there is a row object for the REF.

What is a view/materialized view.
  View
    A view is a logical representation of one or more tables. In essence,
    a view is a stored query. A view derives its data from the tables on which
    it is based, called base tables. Base tables can be tables or other views.
    All operations performed on a view actually affect the base tables. You can
    use views in most places where tables are used.
  Materialized View
    Materialized views are query results that have been stored or "materialized"
    in advance as schema objects. The FROM clause of the query can name tables,
    views, and materialized views. Collectively these objects are called master
    tables (a replication term) or detail tables (a data warehousing term).

What kind of joins do you know?
  Inner joins
    An inner join is a join of two or more tables that returns only rows that
    satisfy the join condition. For example, if the join condition is employees.
    department_id=departments.department_id, then rows that do not satisfy this
    condition are not returned.
  Outer joins
    An outer join returns all rows that satisfy the join condition and also
    returns rows from one table for which no rows from the other table satisfy
    the condition. For example, a left outer join of employees and departments
    retrieves all rows in the employees table even if there is no match in
    departments. A right outer join retrieves all rows in departments even if
    there is no match in employees.
  Cartesian products
    If two tables in a join query have no join condition, then the database
    returns their Cartesian product. Each row of one table combines with each
    row of the other. For example, if employees has 107 rows and departments
    has 27, then the Cartesian product contains 107*27 rows. A Cartesian
    product is rarely useful.

Types of tables (regular, temporary, index-organized etc)
  Oracle Database tables fall into the following basic categories:
  Relational tables: Relational tables have simple columns and are the most common table type.
    A heap-organized table
      does not store rows in any particular order. The CREATE
      TABLE statement creates a heap-organized table by default.
    An index-organized table
      orders rows according to the primary key values. For
      some applications, index-organized tables enhance performance and use disk
      space more efficiently.
    An external table
      is a read-only table whose metadata is stored in the database but
      whose data in stored outside the database.
  Object tables: The columns correspond to the top-level attributes of an object type.
  --
  A table is either permanent or temporary. A permanent table definition and data
  persist across sessions. A temporary table definition persists in the same way as a
  permanent table definition, but the data exists only for the duration of a transaction or
  session. Temporary tables are useful in applications where a result set must be held
  temporarily, perhaps because the result is constructed by running multiple operations.

Are database Indexes useful? What is the role of them?
  An index is an optional structure, associated with a table or table cluster,
  that can sometimes speed data access. By creating an index on one or more
  columns of a table, you gain the ability in some cases to retrieve a small
  set of randomly distributed rows from the table. Indexes are one of many means
  of reducing disk I/O.
  If a heap-organized table has no indexes, then the database must perform a
  full table scan to find a value. For example, without an index, a query of
  location 2700 in the hr.departments table requires the database to search
  every row in every table block for this value. This approach does not scale
  well as data volumes increase.

Why many indexes are not good for performance
Too many indexes will result in more frequent INDEX REBUILD operations which are costly to the database table.
If a table has many indexes and is read heavy and has very less writes (UPDATE / INSERT) instead of SELECT then Indexes will actually speed up the query.
However if there are too many indexes and table has frequent writes then costly Index Rebuild operations will be very frequent.

What does it mean database de-normalization?
todo

Which of SELECT, UPDATE, DELETE, ADDs performance is mostly affected by performance of indexes?
todo

What are ways to increase performance of database
todo
