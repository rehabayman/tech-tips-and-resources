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
3. Mehtod Signature: method name, parameter types and return type.
4. Access Modifiers in Java: [reference](https://www.tutorialspoint.com/java/java_access_modifiers.htm)
    1. Default: visible to the package - no keyword is needed
        - The fields in an interface are implicitly public static final and the methods in an interface are by default public
    2. Private: can only be accessed within the declared class itself
        - Classes and interfaces cannot be private
        - Variables that are declared private can be accessed outside the class, if public getter methods are present in the class
        - Using the private modifier is the main way that an object encapsulates itself and hides data from the outside world
    3. Public: can be accessed from any other class
        - if the public class we are trying to access is in a different package, then the public class still needs to be imported.
    4. Protected: can be accessed only by the subclasses in other package or any class within the package of the protected members' class
        - The protected access modifier cannot be applied to class and interfaces. Methods, fields can be declared protected, however methods and fields in a interface cannot be declared protected.
        - Protected access gives the subclass a chance to use the helper method or variable, while preventing a nonrelated class from trying to use it.
    5. Access Control and Inheritance
        - Methods declared public in a superclass also must be public in all subclasses.
        - Methods declared protected in a superclass must either be protected or public in subclasses; they cannot be private.
        - Methods declared private are not inherited at all, so there is no rule for them.
5. Non-Access Modifiers in Java: [reference](https://www.tutorialspoint.com/java/java_nonaccess_modifiers.htm)
    1. Static: to create a class's variable or method
    2. Abstract:
        - Classes: If it is declared as abstract then the sole purpose is for the class to be extended. A class cannot be final and abstract at the same time.
        - Methods: An abstract method is a method declared without any implementation. The methods body (implementation) is provided by the subclass. Abstract methods can never be final or strict.
    3. Final:
        - Variables: can be explicitly initialized only once. A reference variable declared final can never be reassigned to refer to an different object; used with static to make a class's constant.
    4. Synchronized: used to indicate that a method can be accessed by only one thread at a time
    5. Volatile: 
        - The volatile modifier is used to let the JVM know that a thread accessing the variable must always merge its own private copy of the variable with the master copy in the memory.
        - Accessing a volatile variable synchronizes all the cached copied of the variables in the main memory. Volatile can only be applied to instance variables, which are of type object or private. A volatile object reference can be null.
  
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
    1. FROM
    2. WHERE
    3. GROUP BY
    4. HAVING
    5. SELECT
    6. ORDER BY 

    check this [learnsql article](https://learnsql.com/blog/getting-hang-group-clause/)

  
### PostgreSql
1. Creating a copy of a database: 
    1. Disconnect all other users from the original database
    ```
    SELECT pg_terminate_backend(pg_stat_activity.pid) FROM pg_stat_activity 
    WHERE pg_stat_activity.datname = 'originaldb' AND pid <> pg_backend_pid();
    ```
    2. Create the new DB
    ```
    CREATE DATABASE newdb WITH TEMPLATE originaldb OWNER dbuser;
    ```

2. Retrieve current value of sequence
    ```
    SELECT last_value FROM sequence_name;
    ```