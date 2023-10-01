pipeline {
    agent {
        label 'ajay-node'
    }
    environment {
        //once you sign up for Docker hub, use that user_id here
        registry = "ajaykumarpunati/pythonapp"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'ajay_docker_cred'
        dockerImage = ''
    }
    
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', credentialsId: 'githu-cred', url: 'https://github.com/Research-Associate-Internship/ajay-rac3-docker.git'      
            }
        }
    
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
    
     // Uploading Docker images into Docker Hub
    stage('Upload Image') {
     steps{    
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            }
        }
      }
    }
    
     // Stopping Docker containers for cleaner Docker run
     stage('docker stop container') {
         steps {
            sh 'docker ps -f name=pythonapp -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=pythonapp -q | xargs -r docker container rm'
         }
       }
    
    
    // Running Docker container, make sure port 8081 is opened in 
    stage('Docker Run') {
     steps{
         script {
            dockerImage.run("-p 80:5000 --rm --name pythonapp")
         }
      }
    }
  }
}
