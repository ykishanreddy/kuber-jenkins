pipeline {

  environment {
    registry = "dhananjeyal/dhana"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/ykishanreddy/kuber-jenkins.git'
      }
    }

    stage('Build image') {
      steps{
        sh "docker ps"
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker') {
            dockerImage.push()
          }
        }
      }
    }

 stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(kubeconfigId: 'mykubeconfig',               // REQUIRED

                 configs: 'myweb.yaml', // REQUIRED
                 enableConfigSubstitution: false,
        
                 secretNamespace: 'default',
                 secretName: '',
                 dockerCredentials: [
                       [credentialsId: 'docker'],
                     ]
)
      //    kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }  
          }
  

  }
 }
}
