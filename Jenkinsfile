pipeline {
    agent any
    tools {
        go '1.18.4'
    }
    environment {
        GO114MODULE = 'on'
        //CGO_ENABLED = 0 
        //GOPATH = "${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}"
    
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
    
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
        /*stage('build') {
            steps {
                sh '''
                docker build -f Dockerfile.production -t mathapp-production .
                '''
            }
        }*/
        stage('Docker Build') {
            steps {
                script {
                    
                    def dockerfile = 'Dockerfile.production'
			        docker.build("go-web-docker/mathapp-production:${TAG}", "-f ${dockerfile} .")
                }
            }
        }
	    stage('Pushing Docker Image to Jfrog Artifactory') {
            steps {
                script {
                        docker.withRegistry('https://webappartifactory.jfrog.io/', 'jfrog_credential') 
                        docker.image("go-web-docker/mathapp-production:${TAG}").push()
                        docker.image("go-web-docker/mathapp-production:${TAG}").push("latest")
                }
            }
        }
        stage('checkout SCM') {
            steps {
                script {
                    
                    git 'https://github.com/pratigyanmanjhi/sshpass.git'
			              
                }
            }
        }
        stage('execute anible-playbook') {
            steps {

                ansiblePlaybook credentialsId: 'ansible-node-laptop', disableHostKeyChecking: true, installation: 'ansible', inventory: 'hosts', playbook: 'playbooks/jfrog_login.yaml'
            }
        }
  
      
        

    }
}

