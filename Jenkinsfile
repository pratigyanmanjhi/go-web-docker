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
                    sh 'go mod init'
                    sh 'go version'
                    sh 'go get -u golang.org/x/lint/golint'
                    sh 'github.com/go-sql-driver/mysql'
                 }
                
            }
        }
        
        stage('Build') {
            steps {
                echo 'Compiling and building'
                dir('src') {
                    sh 'go build .' 
                }
                
            }
        }

        stage('Test') {
            steps {
                dir('src') {
                    
                    withEnv(["PATH+GO=${GOPATH}/bin"]){
                        echo 'Running vetting'
                        sh 'go vet .'
                        echo 'Running linting'
                        //sh 'golint .'
                        echo 'Running test'
                        //sh 'cd test && go test -v'
                    }
                
                
                }
            }
        }
        
    }
    /*post {
        always {
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                to: "${params.RECIPIENTS}",
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
            
        }
    }*/  
}
