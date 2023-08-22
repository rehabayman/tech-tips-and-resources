## Caching
1. <b>Cache Stampede</b>: If there are multiple concurrent calls to the cache and they all hit cache miss, the third-party service may get bombarded with multiple concurrent calls. With a high traffic spike on the service all at once, the service may encounter an outage.
    [original article](https://edward-huang.com/tech/software-development/2021/09/14/an-interview-question-that-truly-tests-your-experience-as-a-software-engineer/)  
    [medium article](https://betterprogramming.pub/an-interview-question-that-truly-tests-your-experience-as-a-software-engineer-e188efa4e5b6)

## Concurrency
1. Race Conditions:<br>
        A race condition occurs when two threads access a shared variable at the same time. The first thread reads the variable, and the second thread reads the same value from the variable. Then the first thread and second thread perform their operations on the value, and they race to see which thread can write the value last to the shared variable. The value of the thread that writes its value last is preserved, because the thread is writing over the value that the previous thread wrote.
2. Deadlocks: <br>
        A deadlock occurs when two threads each lock a different variable at the same time and then try to lock the variable that the other thread already locked. As a result, each thread stops executing and waits for the other thread to release the variable. Because each thread is holding the variable that the other thread wants, nothing occurs, and the threads remain deadlocked.
- [ref on the diff between deadlock and race condition](https://learn.microsoft.com/en-US/troubleshoot/developer/visualstudio/visual-basic/language-compilers/race-conditions-deadlocks)