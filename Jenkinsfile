pipeline {
  agent {
          docker{
             image 'misha-agent'
             registryCredentialsId '64588247-86be-4483-9651-9900ec8819c5'
             args '-v /var/run/docker.sock:/var/run/docker.sock -u root '
          }

  }

  stages {

    stage('git clone') {
      steps {
        git 'https://github.com/MISHAALO/boxfuse.git'
       
      }
    }

    stage('Build') {
      steps {
         sh "mvn package"
      }
    }

    stage('Build And Push Docker Image') {
      steps {
        sh 'docker build -t app_image .'
        sh 'docker tag app_image jenkunsHeloo:1.0.0'
        sh  'docker push jenkunsHeloo:1.0.0'

      }
    }
    
    stage('Run docker on devbe') {
      steps {
             sh 'ssh-keyscan -H devbe >> ~/.ssh/known_hosts'
             sh """ssh root@devbe<< EOF 
             docker run -d -p 8081:8080 jenkunsHeloo:1.0.0'
             exit
             EOF"""
      }
    }
  }
}
