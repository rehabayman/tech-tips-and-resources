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
    For reference on how to create the PAT check this [stack overflow answer](https://stackoverflow.com/a/43567591).