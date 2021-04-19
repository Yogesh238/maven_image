pipeline { 
    environment { 
        registry = "yogeshcloudtechner/assignment" 
        registryCredential = 'docker' 
        dockerImage = '' 
    }
    agent any 
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
                
                sh 'docker build -t yogeshcloudtechner/assignment:maven.${BUILD_NUMBER} -f Dockerfile .'
            } 
        }
          stage('Anchore scanner')
        {
            steps {
                
                  sh 'curl -s https://ci-tools.anchore.io/inline_scan-v0.6.0 | bash -s -- -f -d Dockerfile -b .anchore_policy.json yogeshcloudtechner/assignment:maven.${BUILD_NUMBER}'
                      
            }
        }
   stage('Deploy our image') { 
            steps { 
                withDockerRegistry([credentialsId: "docker", url: ""]){
                sh 'docker push yogeshcloudtechner/assignment:maven.${BUILD_NUMBER}' 
                } 
            }
   }
    }
}
