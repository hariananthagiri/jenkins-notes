pipeline {
    agent { 
        node { 
            label 'AGENT-1' 
        } 
    }
    environment {
        GREETING = hello jenkins
    }
    options {
        ansiColor('xterm')
    }
    parameters {
    string(name: 'Name', defaultValue: 'hari krishna', description: 'writeing my name')
    }
}
    // build
    stages {
        stage('Build') {
            steps {
                echo 'Building....'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing2....'
                    sh '''
                    ls -ltr
                    pwd
                    env
                    '''
            }
        }  
        stage('Deploy') {
            steps {
                echo 'Deploying1....'
                sh '''
                    ls -ltr
                    pwd
                    '''
            }
        }
        stage('parameter') {
            steps {
                sh '''
                    echo 'Displaying parameters....'
                    echo "${params.Name}"
                '''
            }
        }
    }
    // post build
    post { 
        always { 
            echo 'It will run whethere job is sucess or not'
        }
        success { 
            echo 'It will run whethere job is sucess'
        }
        failure { 
            echo 'It will run whethere job is failure'
        }    
    }     
}
