### JENKINS
- jenkins is a continues intigration tool
- genrally jenkins stores all of its data on **/var/lib/jenkins** folder
- everything we store in a directory named **workspace**
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
    * options
        1. pipeline scm
        2. SCM - Git
        3. git repository URL
        4. script path(which file to execute from git and specify path/of/file) 
           * ex: Jenkinsfile, catalogue/Jenkinsfile
        5. specify branch name
            * */main
        6. apply & save
* **setup**
* **pipeline scm ==> SCM - Git ==> git repository URL ==> script path-Jenkinsfile ==> branch name-*/main ==> apply & save**
    * we have to provide git repository link here
    * first it will clone all the folders available in repository then it will perform what we execute in it

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
agent { 
    node { 
        label 'labelName' 
    } 
}
```
**example**
agent { node { label 'AGENT-1' } }

5. environment
    * it is used to use environment variables
    * we write it prior to build and under the pipeline
    * to see all environment variables use **printenv** command
    * to see or print particular variable "$variable_name"
    * genrally we use it to store a link 
* **syntax**
```Jenkins
environment {
    env_variable_name = 'value'
}
```
* **example**
```Jenkins
environment {
    GREETING = 'IAM USING JENKINS'
}
```

6. options
    * to enable OR disable any new plugin we use it
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
        timeout(time: '1' unit: 'HOURS')
        disableconcorentbuilds()
    }
```
7. parameters
    * to pass values to the variables based on the datatype we use parameters
* **syntax**
```Jenkins
parameters {
    datatype(name: 'variable_name1',defaultValue: 'variable_value',description: 'write description')
}
```
* to use the particular values in stored in the parameter use as below
```sh
echo "${params.variable_name}"
```
* first time when we run prameters in jenkins it wont work we have use an option called **build with parameters** enable it to do it refresh it first
* execute commands **sh"""dont give commands here"""**
8. input
* to run next stage by takeing approveal or permission
* it is nothing but giveing permission to run from stage to step by provideing yes or abort
* to approve yes or abort to a step we use it
* **stage ===> start or abort ===> steps (it contain commands)**
* **syntax**
```Jenkins
input{
    message "should we continue?"
    ok "yes, we should." 
}
```
* if we give yes it will continue
* if we give abort it will stop
9. when
* it is nothing but giveing a condition
```jenkins
when {
    expression {
        condition
    }
}
```
* it is used at build area below stages
9. script
* to write the groovy script below steps we use it
* **syntax**
```
script {
    write scrript
}
```
* **example**
```
script {
    def browsers = ['chrome', 'firefox']
    for (int i = 0; i < browsers.size(); ++i) {
        echo "Testing the ${browsers[i]} browser"
    }
}   
```
```
script {
    def packageJson = readJSON file: 'package.json'
    packageVersion = packageJson.version
    echo "application version: $packageVersion"
}
```
* in above example dnf(defination) it is used to create a variable store content in it
* we are storeing entire content of package.json file in packageJson variable
* if we wont to refer anything in that file use as below
    * **packageJson.what_you_want**
    * ex: packageJson.version

### POST
    * The post section defines one or more additional steps that are run upon the completion of a Pipeline’s or stage’s run (depending on the location of the post section within the Pipeline).
    * it will be added after stages 
    * post can support any of the following post-condition blocks: 
        * always, changed, fixed, regression, aborted, failure, success, unstable, unsuccessful, and cleanup.
* **deleteDir()**
    * to delete the Directory(in workspace cloned folder) after pipeline is runned or completed 
* **syntax**
```Jenkins
post { 
        always { 
            echo 'It will whethere job is sucess or not'
            deleteDir()
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
    * ex:- **sh ''' commands '''** using single quotes or double quotes
* for single command use **sh'command'**
* **single quotes or double quotes are very importent in jenkins '' & "" to run commands**

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

#### NOTE
* for every stage it will go to root folder to move to seprate folder we have to mention it by below
```sh
cd folder/name
```
* to install terraform in linux meachine centos
```sh
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform
```

* **labauto**
    * it is used to install applications directly from server using root access
    * by this we no need to go to internet and search for application
```sh
sudo su 
labauto
type what you want
select 1,2,3 based on avaiabulity
```

#### NOTE
1. git
* **git ===> repository ===> stores code ===> dont store artifacts**

2. NEXUS
* **nexus ===> repository ===> stores artifacts ===> dont store  code**
* nexus requied minimum 2GB RAM
* sonatype nexus is papular 
* **artifacts** it is nothing but our applicatins (i.e zip file, jar file var files, compiled files e.t.c..)
* **SETUP**
* install a server of centos ===> t3.meadium ===> 30gp2 
* **syntax**
```
sudo su
labauto
give nexus and select install.sh
ip_address:8081
signin
user_name - admin
password - /password/path/copy and check there copy password and paste in nexus
```
* to create repository in nexus
* **settings ===> repositorys ===> create repository(maven2hosted) ===> options ===> finally click on create repository ===> it gives a URL**
* **options**
    * name              - give name
    * version policy    - mixed (snapshot - devlopment env, relese - production env so we are using mixed)
    * layout policy     - permisive
    * deployment policy - allow redploy
* **maven 2 formate**
* it stores everything in a seprate folder parent-folder/child-folder1/child-folder2/child-folder3/child-folder4

* **zip command**
```sh
zip -r file_name what_you_want -x what_you_dont_want
```
* **file_name** with what file_name you want to store your zip file
* **what_you_want** what you want from the folder your ziping
* **what_you_dont_want** what you don't want from the folder your ziping
* **-r** ===> include what_you_want
* **-x** ===> exclude what_you_dont_want
* ./* everything from current folder
* -x "*.zip" next time your ziping the ziped file it wont include it


* **example**
```sh
zip -r catlogue.zip ./* -x ".git" -x "*.zip"
```
* to unzip the ziped file use below command
```sh
unzip ziped_file_name
unzip catalogue.zip
```
* **silent zip log**
* because for jenkins memory is importent
* zip contains more logs that consumes jenkins master memory
* use below command to quite the zip log
```sh
zip -q -r catlogue.zip ./* -x ".git" -x "*.zip"
```
    * q - quite