pipeline {

  environment {
    registry = "ykreddys/kuber"
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
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        sh "chmod 777 /var/lib/jenkins/workspace/kubernet_pipeline/myweb.yaml"
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker') {
            dockerImage.push()
          }
        }
      }
    }

 stage('Deploy to k8s'){
    steps{
         sshagent(['kunedock']){
             sh "scp -o StrictHostKeyChecking=no myweb.yaml root@18.221.106.235:/root/"
             script{
                 try{
                     sh "ssh root@18.221.106.235 kubectl apply -f ."
                 }catch(error){
                     sh "ssh root@18.221.106.235 kubectl create -f ."
                  }
                }
              }
           }
          }  

  }

}
