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
