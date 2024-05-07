REACTIVE PROGRAMMING(SPRING-WEB-FLUX)

Core Features :
1. New Programming Paradigm
2. Asynchronous and non blocking
3. Functional style of writing code
4. Data flow as event driven stream
5. BackPressure on data streams


**Traditional Rest api(Spring web):Thread Per Request model**

1. It supports Thread per request model(one request is served only by one thread)
   for Example: if the application server contains Thread pool which has a size of 20
   now Request 1 will be handled by Thread 1 and it is blocked until we fetch the data from db
   if same happens upto Request 20

   then if 21st request is triggered from user then the server has to wait for any of the threads to complete their task so that particular idle thread will serve request 21.
   this wait time will be sometimes bigger causing application failure.
   In order to nullify this spring web flux came into picture.

**Reactive Programming(Spring web flux):
**

<img width="960" alt="reactiveprogramming" src="https://github.com/saisrinivas12/notes/assets/59176223/1b35c1b4-ef4b-42e6-a526-91a5443985f6">


1. When request comes to the server, a thread will be allocated to handle that request . if it related to a database , it sends an event to the database " Hey Iam not gonna wait for you , you just send me the response when ready "
so that no request will be blocking here

once the response is ready, it sends the complete event and the response to any of the idle thread


**Traditional Rest api vs data driven **
1. In traditional rest api call, users send the request to the application and application gets the data from database and serves it to the user.
   Consider if there is any change happened in the db (like new insert or update happened) ,
   the client will not know unless he makes a new request .
2. In reactive programming we follow publisher - subscriber model where if there is any change happened in the db which is a publisher , it sends an event to the subscriber saying that there is a change happened this way client will be in sync with the updates instead of making new requests every time.

**Backpressure on data streams
**

1. in traditional rest api when we make a db call to the database .. if the returned data is huge which our application is unable to handle due to out of memory ..application down time is more

  in reactive programming there is a concept of backpressure where we inform the database how much data we need to process at a time(in the database driver) so that application downtime will not be there and concurrent.
**

 ** Reactive Programming work flow **


it contains
1. publisher
2. subscriber
3. processor
4. subscription

Publisher : which is responsible for publishing events
![image](https://github.com/saisrinivas12/notes/assets/59176223/0d34ab16-8d5f-455f-a0f9-afc187406988)

Subscriber: which is responsible for subscribing to events 
![image](https://github.com/saisrinivas12/notes/assets/59176223/ecf95c3b-8abf-44b2-b1ce-9db12408fb61)

Processor : which is a combination of both publisher and subscriber
![image](https://github.com/saisrinivas12/notes/assets/59176223/da47e082-6144-434f-9072-c408dc8489af)

Subscription : which is a unique interface between publisher and subscriber 
![image](https://github.com/saisrinivas12/notes/assets/59176223/12120bbb-79b0-4c39-bb1d-6e7eda0087bf)


**Workflow:**

![image](https://github.com/saisrinivas12/notes/assets/59176223/5cbb8d78-4cb4-4ac6-9526-caf16e34f53e)


1. at first subscriber invokes subscribe() method of publisher and then publisher sends an subscription event saying that subscription is successful.

2. then subscriber calls request(n) from subscription interface (n is no of requests )  then publisher calls onNext() method of the subscriber for processing the data. if n is 10 there is a total of n onNext() calls . if the event streaming is complete then publisher will invoke onComplete() method of subscriber or onError() if there is any exception.





