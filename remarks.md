## Tech Tips and Remarks

1. To filter a stream on a field use (distinctBy in the Java Stream API):
    ```
    public static <T> Predicate<T> distinctByKey(final Function<? super T, ?> keyExtractor) {
        final Map<Object, Boolean> seen = new ConcurrentHashMap<>();
        return t -> seen.putIfAbsent(keyExtractor.apply(t), Boolean.TRUE) == null;     
    }
    ```
2. Cherry Pick in Github: it is a command to pick some hash-specified commits to another branch
    ```
    git cherry-pick <commit-hash>
    ```
3. Force Push