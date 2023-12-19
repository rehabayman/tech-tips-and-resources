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

## Multithreading (Java)
1. Enhance this snippet of code
```Java
public class Test  {
   private static Test instance;
   private Test (){}
   public synchronized static getInstance() {
        if (instance == null) {
           instance = new Test();
        }
        return instance;
   }
}

// Enhanced Code
public class Test  {
   private static volatile Test instance;
   private Test (){}
   public static getInstance() {
        /**
         * we added this if statement outside the synchronized
         * block here to make the code concurrent 
         * and that all threads won't be blocked waiting
         **/ 
        if (instance == null) {
           synchronized {
              if (instance == null) { 
                instance = new Test();
              }
            }
        }
        return instance;
   }
}
```

## Web Security
1. SOP, CORS & CSRF: [ref](https://maddevs.io/blog/web-security-an-overview-of-sop-cors-and-csrf/)
   - Websites that use the same scheme, hostname, and port have the same origin
   - SOP (Same Origin Policy): The policy rule suggests that if both websites have the same source, then scripts on one website can only get data from another.
   - CORS (Cross-Origin Resource Sharing): is a mechanism that allows requesting restricted resources from outside the domain of a web page.
      - CORS requests can be divided into simple and preflighted. Simple requests are considered “safe”
         - CORS list of safe methods: HEAD, GET & POST
      - <b>CORS doesn’t block simple requests but blocks reading the response body</b>
      - The browser sends a <b>Preflight</b> request to understand what (non-simple/unsafe) requests the source allows with these headers
         ```
         Request Method: OPTIONS
         Access-Control-Request-Method: DELETE
         ```
         - The browser expects one or more of the following HTTP headers:
         ```
         Access-Control-Allow-Origin (authorized sources)
         Access-Control-Allow-Methods (allowed methods)
         Access-Control-Allow-Headers (allowed headers)
         Access-Control-Allow-Credentials (true/false, do need to pass cookies).
         ```
         - <b>Complex requests CORS first checks and then decides to block or allow them.</b>
   - CSRF (Cross-Site Request Forgery):  is a web attack that exploits loopholes in the SOP policy, and CORS does not block them. The attack consists of the attacker running malicious scripts in the victim's browser, and thus the victim performs unintended actions on his behalf.