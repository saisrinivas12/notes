Jenkins Pipeline :

A continuous delivery (CD) pipeline is an automated expression of your process for getting software from version control right through
to your users and customers. Every change to your software (committed in source control) goes through a complex process on its way to being released.
This process involves building the software in a reliable and repeatable manner, as well as progressing the built software (called a "build") through multiple stages of testing and deployment.

In simple words , getting  the code from version control(github or bitbucket) and run through critical processes
which we defined in the pipeline configuration(Build, Test and Deploy).


Creating a Jenkinsfile for pipeline configuration provides lot of benefits :
1.Automatically creates a Pipeline build process for all branches and pull requests.(from git hub or bitbucket )

2. Code review/iteration on the Pipeline (along with the remaining source code).

3.Audit trail for the Pipeline.

4. Single source of truth  for the Pipeline, which can be viewed and edited by multiple members of the project.


Why do we need pipeline:

1. Basically it supports all sorts of automation patterns. it provides all the automation tools which spans form Continuous Integration to Continuous Delivery.

   Benefits:
1. code : pipelines are developed over code(source control) which developers can easily view or edit.
2. Durable: pipelines can survive both controlled and uncontrolled restarts of the Jenkins Controller.
3. Pausable : pipelines can be paused or wait for human intervention or approval.
4. versatile: it supports real world CI/CD problems like doing parallel stuff.
5. Extensible : it can be easily integrated with other plugins .



"Pipeline as code" can be designed in two approaches
   1. Declarative approach.
   2. Scripting approach .

Basic Building blocks of a pipeline:
1. pipeline : it is a basic building block of CI/CD pipeline which triggers the build test and deploy your application.
2. node : node is machine  which is part of jenkins and able to execute a pipeline .
3. stage: basically this contains the subtasks of what this pipeline gonna do .(build test and deploy)
4. steps: steps is the child for stage where you can define the pipeline execution. eg : build is the stage and what are the steps to be done for build stage


   Declarative approach :
   code snippet :

   pipeline{
       agent any

      stages{
          stage("build"){
            steps{
               // do something
             }
         }
   stage("Test"){
   steps{
   }
   }
   stage("deploy"){
       steps{
   }
   }}
Scripting approach :
One or more nodes will be doing the core task in the pipeline implementation.This is not mandatory.
the code inside the node block will be executed like this.

 1. scheduling the steps inside the node block and then adding into the jenkins queue. Once the executor is free on the node , it will start executing the steps.
 2. creating work space directory, where work can be done on files checked out from source control for the execution.


pipeline example :
pipeline { 
    agent any // tells jenkins to allocate any executor and workspace 
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') { 
            steps { 
                sh 'make' 
            }
        }
        stage('Test'){
            steps {
                sh 'make check' // to execute a shell command 
                junit 'reports/**/*.xml' // to aggregate test reports 
            }
        }
        stage('Deploy') {
            steps {
                sh 'make publish' 
            }
        }
    }
}
   


   
