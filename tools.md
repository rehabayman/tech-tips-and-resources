## AWS
1. Presigned URLs: used to share object in s3 bucket, which by default private.

    1. The object owner can create a presigned URL, using their own security credentials, to grant time-limited permission to download the objects.

    2. When a presigned URL is created for an object, you must provide your security credentials and then specify a bucket name, an object key, an HTTP method (GET to download the object), and an expiration date and time.

    3. The presigned URLs are valid only for the specified duration. If a presigned URL is created using a temporary token, then the URL expires when the token expires, even if the URL was created with a later expiration time.

2. Messaging:

    1. DLQ: Sometimes, a message keeps on failing from being processed. With a <b>Dead Letter Queue</b> it is possible to set a <b>maximum number of retries</b> for a message. When the message is not successfully processed within this maximum number, it will be moved to a DLQ. This is especially interesting for debugging purposes because the message is still available for further analysis.
        - In order to activate this, it is necessary to create a new queue which will serve as a DLQ. 
        - By means of a <b>redrive policy</b>, the DLQ can be linked to the original queue.
        - Process:
            - First, create the DLQ just like you did before with the regular queue.
            - Next, you need to retrieve the ARN (Amazon Resource Name, a unique name within AWS) of the DLQ. This can be retrieved by means of a GetQueueAttributesResponse request.
            - Next, specify the Redrive Policy with a maximum number of retries and the DLQ ARN. By means of a SetQueueAttributesRequest, the Redrive Policy is linked to the original queue. [resource](https://mydeveloperplanet.com/2021/11/23/how-to-use-amazon-sqs-in-a-spring-boot-app/)
    2. FIFO vs Standard Queues:
        - Standard Queues: ensure that the messages are generally delivered in order but can be delivered out of order and more than once
        due to high throughput
        - FIFO Queues: ensure that the messages are delivered only once and in order
    3. How to list all / purge all messages on an sqs locally
        ```
        ## For listing
        aws sqs receive-message --endpoint-url=http://localhost:9324 --queue-url http://localhost:9324/000000000000/queue-name --attribute-names All --message-attribute-names All --max-number-of-messages 10
        ```
        ```
        ## For purging
        aws sqs purge-queue --endpoint-url=http://localhost:9324 --queue-url http://localhost:9324/000000000000/queue-name 
        ```

## Load Balancing:
1. Load balancers (LB) are used to distribute the traffic among our application servers.
2. LB performs health check on each server of our application servers to keep track of which servers to handle the new connection.
3. LB use a range of algorithms to handle this distribution, like:
    - Round Robin: connections are distributed sequentially
    - Weighted Round Robin: builds on the RR algorithm. The admin assigns a weight to each server so that it can be taken into consideration when assigning a connection to a server.
    - Least Connections: dynamic algorithm which monitors the server with the least active connections to direct the new connections to it.
    - Least Time: calculates the response time of each server using a formula that combines the fastest response and the fewest active connections and directs the new connection to the server with the least response time.
    - Source IP Hash: combines the source IP and the destination IP to generate a hash key to allocate a server to this connection. Useful when the client should connect to the same server after the connection is broken.

## Web Workers:
1. They are simple means for web content to run scripts in background threads.
2. The worker thread can perform tasks without interfering with the user interface.
3. Once created, a worker can send messages to the JavaScript code that created it by posting messages to an event handler specified by that code (and vice versa).
4. Workers run in another global context that is different from the current <b>window</b>. Thus, using the <b>window</b> shortcut to get the current global scope (instead of <b>self</b>) within a Worker will return an error.
5. A dedicated worker is only accessible from the script that first spawned it, whereas shared workers can be accessed from multiple scripts.

    --> Web Workers [ref](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers#web_workers_api)

## Infrastructure as Code (IaC):
1. They are simple means for web content to run scripts in background threads.
2. The worker thread can perform tasks without interfering with the user interface.
3. Once created, a worker can send messages to the JavaScript code that created it by posting messages to an event handler specified by that code (and vice versa).
4. Workers run in another global context that is different from the current <b>window</b>. Thus, using the <b>window</b> shortcut to get the current global scope (instead of <b>self</b>) within a Worker will return an error.
5. A dedicated worker is only accessible from the script that first spawned it, whereas shared workers can be accessed from multiple scripts.

    --> Web Workers [ref](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers#web_workers_api)

## Maven
1. Maven is a <b>Build Tool</b> uses <b>pom.xml</b> file for configuration
2. 'run' is not a maven build phase
3. Maven Repository: a place to store maven plugins and dependencies
4. Maven local repository location is: ~/.m2/repository
5. Maven dependecy scope:
    - Indicates the context in which that dependency is used, and it controls the classpath of the project at various stages of the Maven build process.
    - Valid Scopes: 
        - compile: 
3. Commands:
```
    mvn clean install
    -- cleans the project and installs its artifacts into the local repository
```
```
    mvn site
    -- generate a maven site
```