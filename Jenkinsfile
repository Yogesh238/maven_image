pipeline { 
    environment { 
        registry = "yogeshcloudtechner/assignment" 
        registryCredential = 'docker' 
        dockerImage = '' 
    }
    agent any 
    stages { 
        stage ("Quality Gate") { 
   parallel {     
      stage ("Dockerfile") {
         agent {
            docker {
               image 'hadolint/hadolint:latest-debian'
            }
         }
         steps {
            sh 'hadolint microservice1/dockerfile | tee -a      ms1_docker_lint.txt'
         }
         post {
          always {
            archiveArtifacts 'ms1_docker_lint.txt'
          }
      }
  }
}
        }
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
