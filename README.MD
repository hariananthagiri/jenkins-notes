### JENKINS
- jenkins is a continues intigration tool
#### SETUP
* go to jenkins.io select centos
* to install jenkins use below command
```jenkins
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key  
sudo yum install fontconfig java-17-openjdk -y
sudo yum install jenkins -y
sudo systemctl start jenkins
sudo systemctl status jenkins
sudo systemctl enable jenkins
```
* junkins runs at 8080 port number
* browse **IP_ADDRESS:8080** you san see password/path cat password/path copy it and paste it at jenkins install reqied plugins
```jenkins
/var/lib/jenkins/secrets/initialAdminPassword
cat /var/lib/jenkins/secrets/initialAdminPassword
```
* **dashboard** where we lunch directly it is a central option everything is done here
* **configure** we can edit what we configure here
* **build now** to execute the commands

* everything in jenkins is a job there are two types of jobs 
    1. freestyle
    2. pipeline

1. freestyle
    * it is nothing but doing everything in UI
    * it has both advantages and disadvantages
    * we can use agent here by using restrict where this project to be run 
* **disadvantages**
    * here we dont undastand who did what change it is very deficult to restore back to the narmal stage
    * thats the reson why we switched to pipeline now nobody is using freestyle

2. pipeline
    * one jenkins server cannot handle all the load it may hang thats is the reason
    * it contains **master agent or node architecture**
    * jenkins master responsibulity is to distibute the load to multipule agents then overview that job
    * list all nodes and connecting to node as below 
        * **dashboard ===> manage jenkins ===> nodes ===> click on new node ===> name it ===> no.of.executions ===> remote root directory ===> labels ===> launch mathod ===>   host ===> credentials ===> click on save**
    * **no.of.executions** how many jobs that agent should run at a time 
    * **remote root directory** it is the path/of/data related to builts or jobs 
    * **labels** it is nothing but tag names ex:- agent-1
    * **launch mathod** there are two methods here
      1. employ can ask master for work
        * **employe is connecting to master**
      2. when there is a work master can ask employee to do 
        * **master is connecting to employe**
    * **host** given agent ip_address
    * **credentials** give user_name and passsword details of your os we use here centos
* **jenkins is devloped on java we need to install java in both master and agent**
```
sudo yum install fontconfig java-17-openjdk -y
```
- there are two types of pipeline
1. decleretive pipeline
2. scripted pipeline
- decleretive is the latest version pf pipeline in jenkins
* **BASIC SYNTAX**
```Jenkins
pipeline {
    agent any


    stages {
        stage('Build') {
            steps {
                echo "Building...."
            }
        }
        stage('Test') {
            steps {
                echo "Testing...."
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying...."
            }
        }
    }
}
```
* pipeline is of two types
1. pipeline script
2. pipeline script from SCM (Git)

* **multypule sections in pipeline**
1. Agent
    1. any
    * Execute the Pipeline, or stage, on any available agent. 
    For example: agent any
2. none
    * When applied at the top-level of the pipeline block no global agent will be allocated for the entire Pipeline run and each stage section will need to contain its own  agent section. 
    * For example: agent none

3. label
    * Execute the Pipeline, or stage, on an agent available in the Jenkins environment with the provided label. For example: agent { label 'my-defined-label' }
    * Label conditions can also be used: 
    * For example: agent { label 'my-label1 && my-label2' } or agent { label 'my-label1 || my-label2' }
4. node
    * agent { node { label 'labelName' } } behaves the same as agent { label 'labelName' }, but node allows for additional options (such as customWorkspace).
    * to add particular agent use below syntax
```Jenkins
agent { node { label 'labelName' } }
```
**example**
agent { node { label 'AGENT-1' } }

5. options
    * to enable any new plugin we use it
    * this option is used under pipeline
* **syntax**
```Jenkins
options {
    plugin name
}
```
* **example**
```Jenkins
options {
        ansiColor('xterm')
    }
```

* **POST**
    * The post section defines one or more additional steps that are run upon the completion of a Pipeline’s or stage’s run (depending on the location of the post section within the Pipeline).
    * it will be added after stages 
    * post can support any of the following post-condition blocks: 
        * always, changed, fixed, regression, aborted, failure, success, unstable, unsuccessful, and cleanup.
* **syntax**
```Jenkins
post { 
        always { 
            echo 'It will whethere job is sucess or not'
        }
          failure { 
            echo 'It will whethere job is failure'
        }
         success { 
            echo 'It will whethere job is sucess'
        }
         unsuccessful { 
            echo 'It will whethere job is unsucessful'
        }
        aborted { 
            echo 'It will whethere job is aborted'
        }
}
```
* Conventionally, the post section should be placed at the end of the Pipeline.
* Post-condition blocks contain steps the same as the steps section.

* **steps** in steps genrelly we write linux commands
    * ex:- **sh ''' commands '''** using single quotes
* **single quotes are very importent in jenkins **''** **
* **commants**
    * to do commants in Jenkins use **//**

### webhooks
* to automate the code in junkins when we push code to git
#### setup
1. **webhook-setup**
* **login github ===> settings ===> webhook ===> add webhook ===> give the URL to trigger ===> select event ===> add webhook**

* **URL** 
```
jenkins-url/github-webhook/
```
* **jenkins-url ===> http://18.204.198.15:8080**
* **example:-** http://18.204.198.15:8080/github-webhook/

2. **configure-setup**
* **build triggers ===> select github hook trigger for GITScm pooling ===> apply & save**

