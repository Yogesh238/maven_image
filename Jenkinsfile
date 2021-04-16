pipeline { 
    environment { 
        registry = "yogeshcloudtechner/assignment" 
        registryCredential = 'yogeshcloudtechner' 
        dockerImage = '' 
    }
    agent any 
    stages { 
        stage('Cloning our Git') { 
            steps { 
                git branch: 'main', credentialsId: '124fa180-5ea8-4109-94f6-3c65f0efc897', url: 'https://github.com/Yogesh238/maven_image.git' 
            }
        } 
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Deploy our image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
    }
}
