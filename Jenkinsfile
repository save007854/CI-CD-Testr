pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/devops4solutions/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:latest .' 
                sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:latest'
                //sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
      stage('Publish image to Portainer') {
          steps {
              script {
                  // Replace "portainer" with your Jenkins credentials ID for Portainer
                  // Replace "your-portainer-registry-url" with the actual URL of your Portainer registry
                  withDockerRegistry([credentialsId: 'portainer', url: 'docker.io']) {
                      sh 'docker push nikhilnidhi/samplewebapp:latest'
                      // sh 'docker push nikhilnidhi/samplewebapp:$BUILD_NUMBER'
                  }
              }
          }
      }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 nikhilnidhi/samplewebapp"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 nikhilnidhi/samplewebapp"
 
            }
        }
    }
}
    
