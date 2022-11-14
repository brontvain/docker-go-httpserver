pipeline {
        environment { 
        registry = "brontvain/go-httpserver" 
        registryCredential = 'dockerhub-pat' 
        dockerImage = 'docker-gs-ping' 
    }

    agent any 
    stages { 
        stage("Env Variables") {
            steps {
                sh "printenv"
            }
        stage('Cloning Git') { 
            steps { 
                git 'https://github.com/brontvain/docker-go-httpserver' 
            }
        } 
        stage('Building Docker Image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Deploy Docker Image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
    cleanWs()
    }
    }
}
