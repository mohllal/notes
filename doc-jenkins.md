# Jenkins Documentation

[Docs](https://jenkins.io/doc/)!

Date Started: Saturday, August 30, 2019

Date Finished: ongoing

> Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.

## Guided Tour

- *Jenkins pipeline* is a suite of plugins which supports implementing and integrating continuous delivery pipelines into Jenkins.
- A continuous delivery pipeline is an automated expression of your process for getting software from version control right through to your users and customers.
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

  - `-u root`: runs Jenkins as root to allow Jenkins to spawn docker containers to execute stages in your pipelines.
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
