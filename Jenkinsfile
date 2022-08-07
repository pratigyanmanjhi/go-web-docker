pipeline {
    agent any
    tools {
        go '1.18.4'
    }
    environment {
        GO114MODULE = 'on'
        //CGO_ENABLED = 0 
        //GOPATH = "${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}"
    }
    stages {        
        stage('Pre Test') {
            steps {
                 dir('src') {
                    echo 'Installing dependencies'
                    sh 'go version'

                 }
                
            }
        }
        

        stage('Test') {
            steps {
                dir('src') {
                    
                        echo 'Running vetting'

                        echo 'Running test'
   
                }
            }
        }
        stage('build') {
            steps {
                sh '''
                docker build -f Dockerfile.production -t mathapp-production .
                '''
            }
        }
        
    }
     
}

