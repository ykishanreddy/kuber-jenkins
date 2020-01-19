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
          
          kubernetesDeploy configs: 'myweb.yaml', dockerCredentials: [[credentialsId: 'docker', url: 'https://registry.hub.docker.com']], kubeConfig: [path: '/var/lib/jenkins/workspace/kubernet_pipeline/kubeconfig'], kubeconfigId: 'mykubeconfig', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
          
                    //            kubernetesDeploy(
                      //          credentialsType: 'KubeConfig',
                        //        kubeConfig: [path: '/var/lib/jenkins/workspace/kubernet_pipeline/kubeconfig'],
                          //      configs: 'myweb.yaml', 
                            //    dockerCredentials: [
                              //        [credentialsId: 'docker'],
                                //],
      //    kubernetesDeploy(kubeconfigId: 'mykubeconfig',               // REQUIRED

        //         configs: 'myweb.yaml', // REQUIRED
          //       enableConfigSubstitution: false,
        
            //     secretNamespace: 'default',
              //   secretName: '',
                // dockerCredentials: [
                  //     [credentialsId: 'docker'],
                    // ]
//)
      //    kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }  
          }
  

  }
 }
}
