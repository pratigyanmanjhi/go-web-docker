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
	    
	stage('build image for devel') {
	    steps {
		   echo 'Running devel Branch'
                   echo 'BRANCH_NAME' + env.BRANCH_NAME
                   script {
                    if (env.BRANCH_NAME == 'devel') {
                        echo 'Hello from devel branch branch'
                    }  else {
                        sh "echo 'Hello from ${env.BRANCH_NAME} branch!'"
                    }
                    }
            }
	}
	stage('build image for main') {
		when {
		    branch 'main'
		}
	        steps {
                   echo 'Running  main'
			echo 'test for hook'
                }
	}
        stage('build') {
            steps {
                sh '''
                docker build -f Dockerfile.production -t mathapp-productionn_${env.BRANCH_NAME} .
                '''
            }
        }
        /*stage('Docker Build') {
            steps {
                script {
                             echo 'Running test'
                                def dockerfile = 'Dockerfile.production'
			        docker.build("go-web-docker/mathapp-production_${env.BRANCH_NAME}:${TAG}", "-f ${dockerfile} .")
                }
            }
        }
        stage('Pushing Docker Image to Jfrog Artifactory') {
            steps {
                script {
                        docker.withRegistry('https://webappartifactory.jfrog.io/', 'jfrog_credential') { 
                           docker.image("go-web-docker/mathapp-productionn_${env.BRANCH_NAME}:${TAG}").push()
                           docker.image("go-web-docker/mathapp-productionn_${env.BRANCH_NAME}:${TAG}").push("latest")
                        }
                }
            }
        }
        stage('checkout SCM') {
            steps {
               
                    
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/pratigyanmanjhi/sshpass.git'
			              
               
            }
        }
        
      }
      post { 
         always { 
            cleanWs()
         }
      }        
  
      
        

    
}

