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
  def imageLine = 'dockerImage'
  writeFile file: 'anchore_images', text: imageLine
  anchore name: 'my_image_file', engineCredentialsId: 'my_credentials_id', bailOnFail: false
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
