# Jenkins Documentation

[Docs](https://jenkins.io/doc/)!

Date Started: Saturday, August 30, 2019

Date Finished: ongoing

> Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.

## Guided Tour

- *Jenkins pipeline* is a suite of plugins which supports implementing and integrating continuous delivery pipelines into Jenkins.
- A continuous delivery pipeline is an automated expression of the process for getting software from version control right through to the users and customers.
- The definition of a *Jenkins Pipeline* is typically written into a text file called a ***Jenkinsfile*** which in turn is checked into a project’s source control repository.
- Pipelines are made up of multiple steps that allow you to build, test and deploy applications. A *step* is like a single command which performs a single action. When a step succeeds it moves onto the next step. When a step fails to execute correctly the Pipeline will fail.
- The `sh` step is used to execute a shell command in a Pipeline.
- The `retry(num)` step is used to retry other steps it wraps until successful.
- The `timeout(time: num, unit: time_unit)` step is used to exit if a step takes too long.
- ***Wrapper*** steps such as `timeout` and `retry` may contain other steps, including timeout or retry.
- The `post` section is used to run clean-up steps or perform some actions based on the outcome of the Pipeline:
  - `always` will run always whether the Pipeline is succeed or not.
  - `success` will run only if the Pipeline is succeed.
  - `failure` will run only if the Pipeline is failed.
  - `unstable` will run only if the run was marked as unstable.
  - `changed` will run only if the state of the Pipeline has changed.
- The `agent` directive tells Jenkins where and how to execute the Pipeline, or subset thereof, there are a few things `agent` causes to happen:
  - All the steps contained within the block are queued for execution by a Jenkins ***executor*** when it is available.
  - A ***workspace*** is allocated which will contain files checked out from source control as well as any additional working files for the Pipeline.
- Using *Docker* image as an *agent* in the Pipeline allows to define the environment and tools required without having to configure various system tools and dependencies on agents manually.
- The `environment` directive can be used to define environment variables to configure the build or tests differently to run them inside of Jenkins.
- Jenkins can record and aggregate test results using the built-in `junit` step so long as the test runner can output JUnit-style XML test reports.
- A Pipeline that has failing tests will be marked as ***UNSTABLE***, denoted by yellow in the web UI. That is distinct from the ***FAILED*** state, denoted by red.
- Pipeline execution will by default proceed even when the build is ***unstable***. The `skipStagesAfterUnstable` option can be used to skip deployment after test failures.
- The `post` section of a *Pipeline* can be used to add some tasks to perform ***finalization***, ***notification***, or other ***end-of-Pipeline*** tasks.
  
  ```jenkins
  pipeline {
    agent any
    stages {
        stage('No-op') {
            steps {
                sh 'ls'
            }
        }
    }
    post {
        always {
            echo 'One way or another, I have finished'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'I succeeded!'
            slackSend channel: '#ops-room',
                  color: 'good',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
            mail to: 'team@example.com',
                 subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.BUILD_URL}"
        }
        changed {
            echo 'Things were different before...'
        }
    }
  }
  ```

- The most basic continuous delivery pipeline will have, at minimum, three stages which should be defined in a *Jenkinsfile*: ***Build***, ***Test***, and ***Deploy***. Stable *Build* and *Test* stages are an important precursor to any deployment activity.

  ```jenkins
  pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Deploy - Staging') {
            steps {
                 echo 'Deploying --> Staging'
            }
        }
        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }
        stage('Deploy - Production') {
            steps {
                echo 'Deploying --> Production'
            }
        }
    }
  }
  ```

  This kind of pipeline that automatically deploys code all the way through to production can be considered an implementation of ***continuous deployment***.

## Tutorials

- `Jenkinsfile` is the foundation of "Pipeline-as-Code", which treats the continuous delivery pipeline as a part of the application to be versioned and reviewed like any other code.
- Any Pipeline project in Blue Ocean, Jenkins actually creates this as a multibranch Pipeline project behind the scenes.
- The Pipeline stub consists of the basic requirements for a valid Pipeline - i.e. an agent and a stages section, as well as a stage directive.
- The `when` directive (along with their `branch` conditions) determine whether or not the stages (containing these when directive) should be executed. If a `branch` condition’s value (i.e. pattern) matches the name of the branch that Jenkins is running the build from, then the stage that contains this when and branch construct will be executed.

- ```bash
  docker run -u root --rm -d \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean
  ```
  
  This command is used to install Jenkins as a docker container

  - `-u root`: runs Jenkins as root to allow Jenkins to spawn docker containers to execute stages in the pipelines.
  - `-p 8080:8080`: maps port 8080 of the `jenkinsci/blueocean` container to port 8080 on the host machine.
  - `-p 50000:50000`: ***JNLP-based*** Jenkins agents on other machines, which in turn interact with the `jenkinsci/blueocean` container through TCP port `50000` by default(acting as the "master" Jenkins server, or simply Jenkins ***master***).
  - `-v jenkins-data:/var/jenkins_home`: maps the `/var/jenkins_home` directory in the container to the Docker volume with the name `jenkins-data`. If this volume does not exist, then this docker run command will automatically create the volume for you.
  - `-v /var/run/docker.sock:/var/run/docker.sock` allows the `jenkinsci/blueocean` container to communicate with the Docker daemon, which is required if the `jenkinsci/blueocean` container needs to instantiate other Docker containers.

- ```bash
  wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
  sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  sudo apt-get update
  sudo apt-get install jenkins
  ```

  This command is used to install Jenkins through apt.
- Jenkins can store the following types of credentials which are stored in an *encrypted* form on the master Jenkins instance (encrypted by the Jenkins instance ID) and are only handled in Pipeline projects via their ***credential IDs***.
  - ***Secret text*** - a token such as an API token (e.g. a GitHub personal access token).
  - ***Username and password*** - which could be handled as separate components or as a colon separated string in the format username:password (read more about this in Handling credentials).
  - ***Secret file*** - which is essentially secret content in a file,
  - ***SSH Username with private key*** - an SSH public/private key pair,
  - ***Certificate*** - a PKCS#12 certificate file and optional password, or
  - ***Docker Host Certificate Authentication*** credentials.
- Credentials can be added to Jenkins by any Jenkins user who has the ***Credentials > Create permission*** (set through Matrix-based security).
- Jenkins credentials' scopes:
  - ***Global*** - if the credential to be added is for a Pipeline project/item.
  - ***System*** - if the credential to be added is for the Jenkins instance itself to interact with system administration functions, such as email authentication, agent connection, etc.

## Pipeline

- A *continuous delivery* (CD) pipeline is an ***automated expression*** of the process for getting software from version control right through to users and customers. This process involves building the software in a reliable and repeatable manner, as well as progressing the built software (called a "***build***") through multiple stages of *testing* and *deployment*.
- A *Jenkinsfile* can be written using two types of syntax - ***Declarative*** and ***Scripted***.
- features of Pipeline:
  - ***Code***: Pipelines are implemented in code and typically checked into source control, giving teams the ability to edit, review, and iterate upon their delivery pipeline.
  - ***Durable***: Pipelines can survive both planned and unplanned restarts of the Jenkins master.
  - ***Pausable***: Pipelines can optionally stop and wait for human input or approval before continuing the Pipeline run.
  - ***Versatile***: Pipelines support complex real-world CD requirements, including the ability to fork/join, loop, and perform work in parallel.
  - ***Extensible***
- Pipeline can be considered as a form of chaining Freestyle Jobs together to perform sequential tasks.
- A ***Pipeline*** is a user-defined model of a CD pipeline. A Pipeline’s code defines the entire build process, which typically includes stages for building an application, testing it and then delivering it.
- A ***node*** is a machine which is part of the Jenkins environment and is capable of executing a Pipeline.
- A ***stage*** block defines a conceptually distinct subset of tasks performed through the entire Pipeline (e.g. *Build*, *Test* and *Deploy* stages).
- A ***step*** is a single task. Fundamentally, a step tells Jenkins what to do at a particular point in time (or "step" in the process).
- In *Declarative* Pipeline syntax, the `pipeline` block defines all the work done throughout the entire Pipeline.
- In *Scripted* Pipeline syntax, one or more `node` blocks do the core work throughout the entire Pipeline.
- Jenkins uses the name of the Pipeline to create directories on disk. Pipeline names which include spaces may uncover bugs in scripts which do not expect paths to contain spaces.
- To access the ***global variables*** in Jenkins use this format `${VAR_NAME}` with *double quotes* because with *single quotes* the variables don't get processed.
- In a ***Multibranch Pipeline project***, Jenkins automatically discovers, manages and executes Pipelines for branches which contain a Jenkinsfile in source control.
- Pipeline supports adding custom *arguments* which are passed to Docker, allowing users to specify custom *Docker Volumes* to mount, which can be used for caching data on the agent between Pipeline runs.
- Pipeline supports building and running a container from a ***Dockerfile*** in the source repository. Using the `agent { dockerfile true }` syntax will build a new image from a Dockerfile rather than pulling one from Docker Hub.
- By default, Pipeline assumes that ***any configured agent is capable of running Docker-based Pipelines***. Pipeline provides a global option in the *Manage Jenkins page*, and on the Folder level, for specifying which agents (by Label) to use for running Docker-based Pipelines.
- By default, the Docker Pipeline plugin will communicate with a ***local Docker*** daemon, typically accessed through `/var/run/docker.sock`. To select a non-default Docker server, such as with ***Docker Swarm***, the `withServer()` method should be used by passing a URI, and optionally the Credentials ID of a Docker Server Certificate Authentication pre-configured in Jenkins.
- By default the Docker Pipeline integrates assumes the default Docker Registry of ***Docker Hub***. In order to use a ***custom Docker Registry***, users of Scripted Pipeline can wrap steps with the `withRegistry()` method, passing in the custom Registry URL.
- Jenkins can validate, or lint, a ***Declarative Pipeline*** from the command line before actually running it. This can be done using a ***Jenkins CLI*** command or by making an *HTTP POST* request with appropriate parameters.

```bash
# ssh (Jenkins CLI)
# JENKINS_SSHD_PORT=[sshd port on master]
# JENKINS_HOSTNAME=[Jenkins master hostname]
ssh -p $JENKINS_SSHD_PORT $JENKINS_HOSTNAME declarative-linter < Jenkinsfile
```

```bash
# curl (REST API)
# Assuming "anonymous read access" has been enabled on your Jenkins instance.
# JENKINS_URL=[root URL of Jenkins master]
# JENKINS_CRUMB is needed if your Jenkins master has CRSF protection enabled as it should
JENKINS_CRUMB=`curl "$JENKINS_URL/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,\":\",//crumb)"`

curl -X POST -H $JENKINS_CRUMB -F "jenkinsfile=<Jenkinsfile" $JENKINS_URL/pipeline-model-converter/validate
```

- The ***Replay*** in ***Jenkins UI*** feature allows for quick modifications and execution of an existing Pipeline without changing the Pipeline configuration or creating a new commit.

## Blue Ocean

- Blue Ocean is a suit of Jenkins plugins which aims to deliver a great experience around Pipeline and be compatible with any freestyle jobs.
- The first time a Pipeline is created in *Blue Ocean* for a specific Git server. *Blue Ocean* prompts for the server credentials to access its repositories. This is required since *Blue Ocean* can write *Jenkinsfile* to repositories directly.
- A Pipeline can be generated from an existing ***Jenkinsfile*** in source control, or using the *Blue Ocean Pipeline editor* to create a new Pipeline (as a Jenkinsfile that will be committed to source control).
- *Blue Ocean* represents the overall health of a Pipeline/item or one of its repository’s branches using ***weather icons***, which change depending on the number of recent builds that have passed:
  - ***Sunny***: More than 80% of runs passing.
  - ***Partially Sunny***: 61%-80% of runs passing.
  - ***Cloudy***: 41%-60% of runs passing.
  - ***Raining***: 21%-40% of runs passing.
  - ***Storm***: Less than 21% of runs passing.
