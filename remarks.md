## Tech Tips and Remarks

### Java / Spring Boot
1. To filter a stream on a field use (distinctBy in the Java Stream API):
    ```
    public static <T> Predicate<T> distinctByKey(final Function<? super T, ?> keyExtractor) {
        final Map<Object, Boolean> seen = new ConcurrentHashMap<>();
        return t -> seen.putIfAbsent(keyExtractor.apply(t), Boolean.TRUE) == null;     
    }
    ```
  
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
