pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/ykishanreddy/kuber-jenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build("ykreddys/reddy") 
        }
      }
    }

    stage('Push Image') {
      steps{
          //docker.withRegistry('https://registry.hub.docker.com', 'docker') {
            withDockerRegistry(credentialsId: 'docker', url: 'https://registry.hub.docker.com') {
            dockerImage.push()
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
