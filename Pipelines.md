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


   Pipeline Syntax and stages of execution :

**   agent can be defined both in global context or in the stages 
**   pipeline{
   agent any;
   options{
   timeout(1,'SECONDS')//executes the options after agent is allocated 
   }
   }

agent in stage level:

pipeline{
stages{
stage("build"){
agent any 
options{
timeout(1,'SECONDS')//executes the options before agent is allocated 
}
}


Parameters :

1. any : execute the pipeline on any available agent.(agent any)
2. none : when the none is there at the global level, no agent will be allocated to run the pipeline so you need to define own agents inside each stage.
3. label :run any pipeline with the provided label.: agent{ label 'my-label'}
4. node : same like label but it creates custom workspace  agent{ node { label 'my-label'}}
5. //docker and kubernetes we will see later


 common parameters :
 1. customWorkspace : run the pipeline or individual for which customworkspace points to . it can be relative or absolute path.
    pipeline{
    agent {
       node{
         label 'my-label'
          customWorkspace /tmp/var/workspace
    }
    }
    }
2.reuseNode : by default it is set to false. if true we will run the container on the same node instead of a new node completely


post :
it defines what to do after pipeline has been completed it has various options :
always
Run the steps in the post section regardless of the completion status of the Pipeline’s or stage’s run.

changed
Only run the steps in post if the current Pipeline’s run has a different completion status from its previous run.

fixed
Only run the steps in post if the current Pipeline’s run is successful and the previous run failed or was unstable.

regression
Only run the steps in post if the current Pipeline’s or status is failure, unstable, or aborted and the previous run was successful.

aborted
Only run the steps in post if the current Pipeline’s run has an "aborted" status, usually due to the Pipeline being manually aborted. This is typically denoted by gray in the web UI.

failure
Only run the steps in post if the current Pipeline’s or stage’s run has a "failed" status, typically denoted by red in the web UI.

success
Only run the steps in post if the current Pipeline’s or stage’s run has a "success" status, typically denoted by blue or green in the web UI.

unstable
Only run the steps in post if the current Pipeline’s run has an "unstable" status, usually caused by test failures, code violations, etc. This is typically denoted by yellow in the web UI.

unsuccessful
Only run the steps in post if the current Pipeline’s or stage’s run has not a "success" status. This is typically denoted in the web UI depending on the status previously mentioned (for stages this may fire if the build itself is unstable).

cleanup
runs after pipeline execution irrespective of the pipeline status


Example:

pipeline{
agent any 
stages{ 
   stage("build"){
   steps{
   }
   }
   }
   post{
   success{
   sh 'echo"successfully finished the pipeline"'
   }
   }

   
