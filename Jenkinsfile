pipeline { 
    environment { 
        registry = "yogeshcloudtechner/assignment" 
        registryCredential = 'docker' 
        dockerImage = '' 
    }
    agent any 
     options {
        timeout(time: 10, unit: 'MINUTES')   // timeout on whole pipeline job
    }
    stages { 
         stage ("Docker linting") {
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
                    dockerImage = docker build -t registry + ":maven.$BUILD_NUMBER" -f Dockerfile
                    

                }
            } 
        }
          stage('Anchore scanner')
        {
            steps {
                
                  sh 'curl -s https://ci-tools.anchore.io/inline_scan-latest | bash -s -- -f -d Dockerfile -b .anchore-policy.json yogeshcloudtechner/assignment:maven.${BUILD_NUMBER}'
                      
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
