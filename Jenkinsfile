pipeline { 
    environment { 
        registry = "yogeshcloudtechner/assignment" 
        registryCredential = 'docker' 
        dockerImage = '' 
    }
    agent any 
    stages { 
               
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":maven.$BUILD_NUMBER" 
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
