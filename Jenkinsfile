pipeline { 
    environment { 
        registry = "yogeshcloudtechner/assignment" 
        registryCredential = 'docker' 
        dockerImage = '' 
    }
    agent any 
    stages { 
         stage ("Dockerfile") {
         agent {
            docker {
               image 'hadolint/hadolint:latest-debian'
            }
         }
         steps {
            sh 'hadolint Dockerfile | tee -a      ms1_docker_lint.txt'
             sh 'cat ms1_docker_lint.txt'
         }
         post {
          always {
            archiveArtifacts 'ms1_docker_lint.txt'
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
        stage('Anchore scanner')
        {
            steps {
                   writeFile file: 'anchore_images', text: registry + ":maven.$BUILD_NUMBER"
anchore engineCredentialsId: 'a724dbba-30d6-4446-8f78-48b72ab861c3', engineurl: 'http://18.213.150.82:8228/v1', name: 'anchore_images'
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
