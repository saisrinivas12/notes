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


   Parameters:
   1. it defines the list of inputs which user should provide while triggering the pipeline.

string
A parameter of a string type, for example: parameters { string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') }.

text
A text parameter, which can contain multiple lines, for example: parameters { text(name: 'DEPLOY_TEXT', defaultValue: 'One\nTwo\nThree\n', description: '') }.

booleanParam
A boolean parameter, for example: parameters { booleanParam(name: 'DEBUG_BUILD', defaultValue: true, description: '') }.

choice
A choice parameter, for example: parameters { choice(name: 'CHOICES', choices: ['one', 'two', 'three'], description: '') }. The first value is the default.

password
A password parameter, for example: parameters { password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'A secret password') }.

codesnippet:

pipeline{
agent any
parameters{
string(name:'DEPLOY_ENV',defaultValue:'staging',description:'this is test')
text(name:'DEPLOY_TEXT', defaultValue:'',description:'this is test')
booleanParam(name:'DEBUG_TEST', defaultValue:true , description:'')
choice(name:'debug_choice',choices:['one','two','three'], description:'')
password(name:'debug_password',defaultValue:'SECRET',description:'')
}

stages{
stage('Example Build'){
steps{
echo 'string value ${params.DEPLOY_ENV}
echo 'text value ${params.DEPLOY_TEXT}
echo 'boolean value ${params.DEBUG_TEST)
echo ' choice value ${params.debug_choice}
echo 'password value ${params.debug_password}
}
}

}
}


triggers:

automated way for retriggering the pipelines 
It has three ways for retriggering the pipelines


1. cron : cronexpression which makes it to retrigger the pipeline based on the provided value
          triggers{
             cron('H */4 * * 1-5')
           }
2.pollSCM: automated way for checking the source control whether any source code has been changed based on the provided value .
          triggers{
            pollSCM('H */4 * * 1-5')
   }
3.upstream : accepts a comma separated jobs and a threshold. if any job completes with in the given threshold then pipeline will be retriggered.
     triggers{
   upstream(upstreamProjects: 'job1,job2', threshold: hudson.model.Result.SUCCESS)
   }

tools:
it defines the section which makes the things to autoinstall and put it on the PATH. this is ignored if agent none is specified.

example:
pipeline{
agent any

tools{
'apache-maven 0.0.1'
}
stages{
stage('Example build'){
steps{
echo 'mvn --version'
}
}
}
}


input :
input block will prompt users to give the input . it should be evaluated before any stage or when condition being evaluated .

configurations for input:

message :
  this should be like what should be shown when clicking on submit(before clicking on submit)
id:
optional identifier default is based on the stage name

ok:
optional value for the input form ok button

submitter:
comma separated list of users and external group names to submit the form.

submitterParameter:
An optional name of an environment variable to set with the submitter name, if present.

parameters:
an optional list of parameters which we prompt users to provide 



example:
pipeline{
agent any
stages{
stage('Example Build'){
input{
    message "should we continue?"
    ok "yes, we should do ",
    submitter "alice,bob"
    parameters{
        string(name:'PERSON',defaultValue:'Mr Jenkins',description:'who should i say hello to')
    }
}
steps{
 echo 'Hello welcome ${param.PERSON}'

}
}
}
}

when
The when directive allows the Pipeline to determine whether the stage should be executed depending on the given condition. The when directive must contain at least one condition. If the when directive contains more than one condition, all the child conditions must return true for the stage to execute. This is the same as if the child conditions were nested in an allOf condition (refer to the examples below). If an anyOf condition is used, note that the condition skips remaining tests as soon as the first "true" condition is found.

More complex conditional structures can be built using the nesting conditions: not, allOf, or anyOf. Nesting conditions may be nested to any arbitrary depth.

Built-in Conditions
branch
Execute the stage when the branch being built matches the branch pattern (ANT style path glob) given, for example: when { branch 'master' }. Note that this only works on a multibranch Pipeline.

The optional parameter comparator may be added after an attribute to specify how any patterns are evaluated for a match:

EQUALS for a simple string comparison

GLOB (the default) for an ANT style path glob (same as for example changeset)

REGEXP for regular expression matching

For example: when { branch pattern: "release-\\d+", comparator: "REGEXP"}

buildingTag
Execute the stage when the build is building a tag. For example: when { buildingTag() }

changelog
Execute the stage if the build’s SCM changelog contains a given regular expression pattern, for example: when { changelog '.*^\\[DEPENDENCY\\] .+$' }.

changeset
Execute the stage if the build’s SCM changeset contains one or more files matching the given pattern. Example: when { changeset "**/*.js" }

The optional parameter comparator may be added after an attribute to specify how any patterns are evaluated for a match:

EQUALS for a simple string comparison

GLOB (the default) for an ANT style path glob case insensitive (this can be turned off with the caseSensitive parameter).

REGEXP for regular expression matching

For example: when { changeset pattern: ".TEST\\.java", comparator: "REGEXP" } or when { changeset pattern: "*/*TEST.java", caseSensitive: true }

changeRequest
Executes the stage if the current build is for a "change request" (a.k.a. Pull Request on GitHub and Bitbucket, Merge Request on GitLab, Change in Gerrit, etc.). When no parameters are passed the stage runs on every change request, for example: when { changeRequest() }.

By adding a filter attribute with parameter to the change request, the stage can be made to run only on matching change requests. Possible attributes are id, target, branch, fork, url, title, author, authorDisplayName, and authorEmail. Each of these corresponds to a CHANGE_* environment variable, for example: when { changeRequest target: 'master' }.

The optional parameter comparator may be added after an attribute to specify how any patterns are evaluated for a match:

EQUALS for a simple string comparison (the default)

GLOB for an ANT style path glob (same as for example changeset)

REGEXP for regular expression matching

Example: when { changeRequest authorEmail: "[\\w_-.]+@example.com", comparator: 'REGEXP' }

environment
Execute the stage when the specified environment variable is set to the given value, for example: when { environment name: 'DEPLOY_TO', value: 'production' }.

equals
Execute the stage when the expected value is equal to the actual value, for example: when { equals expected: 2, actual: currentBuild.number }.

expression
Execute the stage when the specified Groovy expression evaluates to true, for example: when { expression { return params.DEBUG_BUILD } }.

When returning strings from your expressions they must be converted to booleans or return null to evaluate to false. Simply returning "0" or "false" will still evaluate to "true".
tag
Execute the stage if the TAG_NAME variable matches the given pattern. For example: when { tag "release-*" } If an empty pattern is provided the stage will execute if the TAG_NAME variable exists (same as buildingTag()).

The optional parameter comparator may be added after an attribute to specify how any patterns are evaluated for a match:

EQUALS for a simple string comparison,

GLOB (the default) for an ANT style path glob (same as for example changeset), or

REGEXP for regular expression matching.

For example: when { tag pattern: "release-\\d+", comparator: "REGEXP"}

not
Execute the stage when the nested condition is false. Must contain one condition. For example: when { not { branch 'master' } }

allOf
Execute the stage when all of the nested conditions are true. Must contain at least one condition. For example: when { allOf { branch 'master'; environment name: 'DEPLOY_TO', value: 'production' } }

anyOf
Execute the stage when at least one of the nested conditions is true. Must contain at least one condition. For example: when { anyOf { branch 'master'; branch 'staging' } }

triggeredBy
Execute the stage when the current build has been triggered by the param given. For example:

when { triggeredBy 'SCMTrigger' }

when { triggeredBy 'TimerTrigger' }

when { triggeredBy 'BuildUpstreamCause' }

when { triggeredBy cause: "UserIdCause", detail: "vlinde" }











when will be executed after the options,input and agent . this can be changed with certain params

having beforeAgent to true will make when to execute before  agent 

Example:

pipeline{
agent none

stages{
stage('Example deploy'){
  agent{
  label 'My-label'
  }
  when{
  beforeAgent true
  branch 'production'
  }
  steps{
  echo 'deploying'
  }
  }
  }
  }


having beforeInput to true will make when to execute before Input.

example:
pipeline{
agent any 

stages{
stage('Example deploy'){
    input{
    message 'should we continue?'
    ok 'yes we should'
    submitter 'alice,bob'
    parameters{
       string(name:'PERSON', defaultValue:'Mr Jenkins', description:'who should i say hello to ?')
       }
      }
      when{
      beforeInput true
      branch 'production'
      }
      steps{
      echo 'deploying'
      }
      }
      }
      }
    }

    
beforeOptions example:

    pipeline {
    agent none
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            when {
                beforeOptions true
                branch 'testing'
            }
            options {
                lock label: 'testing-deploy-envs', quantity: 1, variable: 'deployEnv'
            }
            steps {
                echo "Deploying to ${deployEnv}"
            }
        }
    }
}



sequential and parallel stages  we will see an example


Matrix:

it is a multidimensional matrix which contains name value combinations. we will call this as cells .
each cell can contain any no of stages depending on conf of cell.

to define a matrix we should know axes.


axes:
The axes section specifies one or more axis directives. Each axis consists of a name and a list of values. All the values from each axis are combined with the others to produce the cells.

Example:
matrix {
    axes {
        axis {
            name 'PLATFORM'
            values 'linux', 'mac', 'windows'
        }
    }
    // ...
}

matrix {
    axes {
        axis {
            name 'PLATFORM'
            values 'linux', 'mac', 'windows'
        }
        axis {
            name 'BROWSER'
            values 'chrome', 'edge', 'firefox', 'safari'
        }
    }
    // ...
}

excludes (optional)
The optional excludes section lets authors specify one or more exclude filter expressions that select cells to be excluded from the expanded set of matrix cells (aka, sparsening). Filters are constructed using a basic directive structure of one or more of exclude axis directives each with a name and values list.

The axis directives inside an exclude generate a set of combinations (similar to generating the matrix cells). The matrix cells that match all the values from an exclude combination are removed from the matrix. If more than one exclude directive is supplied, each is evaluated separately to remove cells.

When dealing with a long list of values to exclude, exclude axis directives can use notValues instead of values. These will exclude cells that do not match one of the values passed to notValues.

   
