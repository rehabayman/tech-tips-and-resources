## Tech Tips and Remarks

### Java / Spring Boot
1. To filter a stream on a field use (distinctBy in the Java Stream API):
    ```
    public static <T> Predicate<T> distinctByKey(final Function<? super T, ?> keyExtractor) {
        final Map<Object, Boolean> seen = new ConcurrentHashMap<>();
        return t -> seen.putIfAbsent(keyExtractor.apply(t), Boolean.TRUE) == null;     
    }
    ```
2. Java supports covariant return types for overridden methods. This means an overridden method may have a more specific return type. That is, as long as the new return type is assignable to the return type of the method you are overriding, it's allowed. For reference check this [stack overflow answer](https://stackoverflow.com/a/14694885).
3. Mehtod Signature: method name & parameter list (types & number).
    - can have 2 functions with the same name, different parameters' list and different return types.
4. Access Modifiers in Java: [reference](https://www.tutorialspoint.com/java/java_access_modifiers.htm)
    1. Default: visible to the package - no keyword is needed
        - The fields in an interface are implicitly public static final and the methods in an interface are by default public
    2. Private: can only be accessed within the declared class itself
        - Classes and interfaces cannot be private <b>Excepet for inner classes, they can be private static</b>
    3. Public: can be accessed from any other class
    4. Protected: can be accessed only by the subclasses in other package or any class within the package of the protected members' class
    5. Access Control and Inheritance
        - Methods declared public in a superclass also must be public in all subclasses.
        - Methods declared protected in a superclass must either be protected or public in subclasses; they cannot be private.
        - Methods declared private are not inherited at all, so there is no rule for them.
5. Non-Access Modifiers in Java: [reference](https://www.tutorialspoint.com/java/java_nonaccess_modifiers.htm)
    1. Static: to create a class's variable or method
    2. Abstract:
        - Classes: If it is declared as abstract then the sole purpose is for the class to be extended. A class cannot be final and abstract at the same time.
        - Methods: An abstract method is a method declared without any implementation. Abstract methods can never be final or strict.
    3. Final:
        - Variables: can be explicitly initialized only once. Used with static to make a class's constant.
    4. Synchronized: used to indicate that a method can be accessed by only one thread at a time
    5. Volatile: 
        - The volatile modifier is used to let the JVM know that a thread accessing the variable must always merge its own private copy of the variable with the master copy in the memory.
        - Accessing a volatile variable synchronizes all the cached copied of the variables in the main memory. Volatile can only be applied to instance variables, which are of type object or private. A volatile object reference can be null.
6. To convert a List to Set while maintaining order use LinkedHashSet as 

    <em>it maintains a doubly-linked list running through all of its entries. This linked list defines the iteration ordering, which is the insertion-order. Note that insertion order is not affected if an element is re-inserted into the set. (An element e is reinserted into a set s if s.add(e) is invoked when s.contains(e) would return true immediately prior to the invocation.)</em>
[reference](https://stackoverflow.com/a/20095304)

7. Diff between @Autowired and @Qualifier
    - @Autowired: to autowire(or search) by-type.
    - @Qualifier: [ref](https://stackoverflow.com/a/48134386)
        - to autowire(or search) by-name.
        - the annotation is used to resolve the autowiring conflict, when there are multiple beans of same type.
        - can be used on any class annotated with @Component or on methods annotated with @Bean. This annotation can also be applied on constructor arguments or method parameters. 

        [definition reference](https://stackoverflow.com/a/50829233)

8. Supplier Interface: It represents a function which does not take in any argument but produces a value of type T.
    - The lambda expression assigned to an object of Supplier type is used to define its get() which eventually produces a value
    - The Supplier interface consists of only one function: get()
    - Example:
    ```
    // This function returns a random value.
    Supplier<Double> randomValue = () -> Math.random();
    System.out.println(randomValue.get());
    ```

9. We can use @Scope("prototype") on the component to make the return of the qualifier not the sigleton object but a new object every time I use the qualifier annotation <i>---> needs revist</i>

10. Setting JVM timezone in spring boot app:
    ```
    @SpringBootApplication
    public class Application {

        @Value("${country.timezone}")
        private String timeZone;

        public static void main(final String[] args) {
            SpringApplication.run(Application.class, args);
        }

        // this is how we change jvm timezone
        @PostConstruct
        public void init() {
            TimeZone.setDefault(TimeZone.getTimeZone(this.timeZone));
        }

    }
    ```
11. Configuring hibernate to control timezone:
    In the application.properties file use:
    ```
    spring.jpa.properties.hibernate.jdbc.time_zone=UTC
    ```
    This way Hibernate automatically translates the LocalDateTime from your application timezone (eg. GMT+2) into GMT (=UTC) when <b>communicating with the database</b>

12. Converting a list of object to a map with a key and the value is the object itself
    ```
    employeeList.stream().collect(Collectors.toMap(Employee::getId, Function.identity()));
    ```
13. Transaction Propagation and Isolation in Spring (@Transactional): [reference](https://www.baeldung.com/spring-transactional-propagation-isolation)
    - @Transactional allows us to set propagation, isolation, timeout, read-only, and rollback conditions for our transaction.
    - We can put the annotation on definitions of interfaces (not recommended but acceptable in @Repository), classes, or directly on methods. They override each other according to the priority order; from lowest to highest we have: interface, superclass, class, interface method, superclass method, and class method.
    - Spring applies the class-level annotation to all public methods of this class that we did not annotate with @Transactional. However, if we put the annotation on a private or protected method, Spring will ignore it without an error.
    - Transaction Propagation types:
        - REQUIRED is the default propagation. Spring checks if there is an active transaction, and if nothing exists, it creates a new one. Otherwise, the business logic appends to the currently active transaction.
        - SUPPORTS, Spring first checks if an active transaction exists. If a transaction exists, then the existing transaction will be used. If there isn't a transaction, it is executed non-transactional
        ```
        @Transactional(propagation = Propagation.SUPPORTS)
        ```
        - MANDATORY, if there is an active transaction, then it will be used. If there isn't an active transaction, then Spring throws an exception
        - NEVER propagation, Spring throws an exception if there's an active transaction
        - NOT_SUPPORTED, If a current transaction exists, first Spring suspends it, and then the business logic is executed without a transaction
        <i>The JTATransactionManager supports real transaction suspension out-of-the-box.</i>
        - REQUIRES_NEW, Spring suspends the current transaction if it exists, and then creates a new one
        - NESTED, if a transaction exists, it marks a save point. This means that if our business logic execution throws an exception, then the transaction rollbacks to this save point. If there's no active transaction, it works like REQUIRED.
        <i>DataSourceTransactionManager supports this propagation out-of-the-box.</i>

14. Caching in Hibernate:
    - First Level Cache
        - associated with the Session object.
        - Hibernate first level cache is enabled by default and there is no way to disable it.
    - Second Level Cache [[Hazelcast]](https://hazelcast.com/glossary/hibernate-second-level-cache/)
        - shares cached data across sessions, so all sessions/users can benefit from the cached data, even for data that was inserted by another session, and even if the session that inserted the data into the second-level cache closes
        - disabled by default but we can enable it through configuration.
    - Query Level Cache
        - it caches only identifier values and results of value type.
        - it should always be used in conjunction with the second-level cache.


15. Generics: like templates in C++ <i>"To Be Continued"</i>
    - The <b>Object</b> is the superclass of all other classes, and Object reference can refer to any object. These features <b>lack type safety</b>. Generics add that type of safety feature.
    - Types:
        - Generic Method: It is exactly like a normal function, however, a generic method has type parameters that are cited by actual type. The compiler takes care of the type safety which enables programmers to code easily since they do not have to perform long, individual type castings.
    
  
### Github
1. Cherry Pick in Github: it is a command to pick some hash-specified commits to another branch
    ```
    git cherry-pick <commit-hash>
    ```
2. Git PAT (personal access token):
    From August 13, 2021, GitHub is no longer accepting account passwords when authenticating Git operations. You need to add a PAT (Personal Access Token) instead, and you can follow the below method to add a PAT on your system.
    For reference on how to create the PAT check this [stack overflow answer](https://stackoverflow.com/a/68781050).
3. Force Push vs. Normal Push Github:
    You only force a push when you need to replace the remote history by your local history.
    For reference check this [stack overflow answer](https://stackoverflow.com/a/43567591).
4. Git Rebase vs Merge:
    Merging is a safe option that preserves the entire history of your repository, while rebasing creates a linear history by moving your feature branch onto the tip of main.
    For reference check this [bitbucket article](https://www.atlassian.com/git/tutorials/merging-vs-rebasing1).
  
### SQL
1. WITH clause (also known as common table expression (CTE)):
    1. Often interchangeably called CTE or subquery refactoring, a WITH clause defines a temporary data set whose output is available to be referenced in subsequent queries.
    2. The general sequence of steps to execute a WITH clause is:
        1. Initiate the WITH
        2. Specify the expression name for the to-be-defined query.
        3. Optional: Specify column names separated by commas.
        4. After assigning the name of the expression, enter the AS command. The expressions, in this case, are the named result sets that you will use later in the main query to refer to the CTE.
        5. Write the query required to produce the desired temporary data set.
        6. If working with more than one CTEs or WITH clauses, initiate each subsequent one separated by a comma and repeat steps 2-4. Such an arrangement is also called a nested WITH clause.
        7. Reference the expressions defined above in a subsequent query using SELECT, INSERT, UPDATE, DELETE, or MERGE
    3. Use Cases:
        1. Improves Code Readability & Maintainability
        2. Alternative to a View
        3. Overcome Statement Limitations – CTEs help overcome constraints such as SELECT statement limitations, for example, performing a GROUP BY using non-deterministic functions
2. SQL SELECT statement always executes in this order: 
    1. FROM & JOINs
    2. WHERE
    3. GROUP BY
    4. HAVING
    5. SELECT
    6. ORDER BY 
    7. Limit / Offset

    check these [learnsql article](https://learnsql.com/blog/getting-hang-group-clause/), [sql bolt article](https://sqlbolt.com/lesson/select_queries_order_of_execution)

3. Join Strategies: 
    1. Nested Loop: [MS Docs](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008/ms191318(v=sql.100))
        - What? The query optimizer selects one table as the outer (first) table and the other as the inner table. The outer table is scaned row by row. For each row, it also scans the inner table, looking for matching rows.
        - When? This join type is very efficient if one of the tables is small (or has been filtered so that it has only a few qualifying rows) and if the other table has an index on the column that joins the tables. The optimizer typically chooses the small table as its first input because this is the fastest method of executing the nest-loop algorithm.
        - Needs? For the nested-loop strategy to be effective, the inner table needs an index on the join column. 
        - <b>PS</b>. if the join between the tables isn't based on equality, the optimizer chooses a nested-loop join, regardless of the tables' sizes and indexing schema
    2. Merge Join:[MS Docs](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008/ms190967(v=sql.100))
        - What? the Merge Join operator gets a row from each input and compares them
        - When? If the two join inputs are not small but are sorted on their join column. Also, If both join inputs are large and the two inputs are of similar sizes, a <b>merge join with prior sorting</b> and a <b>hash</b> join offer similar performance.
        - Needs? both inputs to be sorted on the merge columns, which are defined by the equality (ON) clauses of the join predicate.
        - <b>PS</b>. Merge join itself is very fast, but it can be an expensive choice if sort operations are required. However, if the data volume is large and the desired data can be obtained presorted from existing B-tree indexes, merge join is often the fastest available join algorithm
    3. Hash Join: [MS Docs](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008/ms189313(v=sql.100))
        - What? The hash join has two inputs: the build input and probe input. The query optimizer assigns these roles so that the smaller of the two inputs is the build input. Hash joins are used for many types of set-matching.
        - When? large, unsorted, nonindexed inputs. They are useful for intermediate results in complex queries
        - Types:
            - In-Memory Hash Join:

4. UNNEST function:
    - The UNNEST function takes an ARRAY and returns a table with a row for each element in the ARRAY.
    - Syntax:
        ```sql
            UNNEST(ARRAY) [WITH OFFSET]

            -- example
            SELECT * 
            FROM UNNEST([1, 2, 2, 5, NULL]) AS unnest_column WITH OFFSET AS `offset`
        ```
    - [reference](https://count.co/sql-resources/bigquery-standard-sql/unnest)
  
### PostgreSql
1. Creating a copy of a database: 
    1. Disconnect all other users from the original database
    ```sql
    SELECT pg_terminate_backend(pg_stat_activity.pid) FROM pg_stat_activity 
    WHERE pg_stat_activity.datname = 'originaldb' AND pid <> pg_backend_pid();
    ```
    2. Create the new DB
    ```sql
    CREATE DATABASE newdb WITH TEMPLATE originaldb OWNER dbuser;
    ```

2. Retrieve current value of sequence
    ```sql
    SELECT last_value FROM sequence_name;
    ```

3. Indexes in PostgreSql: [Docs](https://www.postgresql.org/docs/current/indexes-types.html)
    1. Create an index:
        ```sql
        CREATE INDEX (name_of_index) on (name_of_table) USING (index_type) (name_of_column(s));
        ```
    2. Types: B-Tree, Hash, GIN, BRIN, SP-GiST & GiST.
        - B-Tree:
            - The query planner will consider using this index whenever an indexed column is involved in a comparison using one of these operators:
                ```
                <   <=   =   >=   >
                ```
            - Other operators: 
                - BETWEEN and IN
                - IS NULL or IS NOT NULL
                - LIKE and ~ <i>if the pattern is a constant and is anchored to the beginning of the string — for example, col LIKE 'foo%' or col ~ '^foo', but not col LIKE '%bar'</i>
                - ILIKE and ~*, but only if the pattern starts with characters that are not affected by upper/lower case conversion.
            - B-tree indexes can also be used to retrieve data in sorted order. This is not always faster than a simple scan and sort, but it is often helpful.
        - Hash:
            - Stores a 32-bit hash code derived from the value of the indexed column.
            - Can only handle simple equality comparisons (= operator)
        - GIN:
            - inverted indexes which are appropriate for data values that contain multiple component values, such as arrays
            - An inverted index contains a separate entry for each component value, and can efficiently handle queries that test for the presence of specific component values.
            - The standard distribution of PostgreSQL includes a GIN operator class for arrays, which supports indexed queries using these operators:
                ```
                <@   @>   =   &&
                ```

    

    
## PHP
1. Sanitizing User Input:
    - PHP filters are used to validate and sanitize external input. [check this w3schools article](https://www.w3schools.com/php/php_filter.asp)

## Laravel
1. Options when creating a model

    1. Create a migration with the model
    ```
    php artisan make:model <modelName> -m
    ```
    2. Create a resourcful controller with the model
    ```
    php artisan make:model <modelName> -mcr
    ```
    3. Specify this controller is API controller (generates only controller with 5 methods cuz APIs don't have create and edit forms)
    ```
    php artisan make:model <modelName> -mcr --api
    ```
2. Delay a job or listener after a transaction is commited: [ref](https://arunas.dev/how-to-delay-laravel-jobs-and-listeners-within-database-transactions/)
    1. Job: can use ```afterCommit``` method
        ```
        DB::transaction(function () use ($data) {
            // Perform database queries here

            dispatch(new MyJob($data))->afterCommit();
            
            // alternatively, if the job uses the Dispatchable trait:
            // MyJob::dispatch($data)->afterCommit();

            // Perform other operations that could potentially fail
            // and roll back the transaction.
        });
        ```
        ```
        DB::transaction(function () use ($data) {
            // Perform database queries here

            DB::afterCommit(function () {
                dispatch(new MyJob($data));
            });

            // Perform other operations that could potentially fail
            // and roll back the transaction.
        });
        ```
    2. Listener: can use ```$afterCommit``` property
        ```
        $afterCommit = true;
        ```
        - It does not matter whether the listener is synchronous or asynchronous