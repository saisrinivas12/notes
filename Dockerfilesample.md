FROM baseimage // this is where you will kow what operating system you need 
WORKDIR /usr/lib     //this specifies the working directory in ubuntu operating system 
RUN openjdk,log4j.jar   //this will install all the dependencies which is required to run your application
CMD ["java","test"]   // this will run when you run your image 


EXPOSE portNo // this will expose port of your ubunu where your application is running (basically your application is running on 8080)
