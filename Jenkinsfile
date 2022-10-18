pipeline{
    agent any

       environment {
            DEPLOY = "${env.BRANCH_NAME == "python-dramed" || env.BRANCH_NAME == "master" ? "true" : "false"}"
            NAME = "${env.BRANCH_NAME == "python-dramed" ? "example" : "example-staging"}"
            //def mavenHome =  tool name: "maven:11.0.16", type: "maven"
            //def mavenCMD = "${mavenHome}/usr/share/maven"
            VERSION = "${env.BUILD_ID}"
            REGISTRY = 'eagunuworld/python-dramed'
            REGISTRY_CREDENTIAL = 'eagunuworld_credentials'
          }

    stages {


      // stage('Example') {
      //      if (env.BRANCH_NAME == 'python-dramed') {
      //            echo 'I only execute on this environment $env.BRANCH_NAME'
      //          } else {
      //               echo 'I execute elsewhere'
      //              }
      //           }

         stage('checking... python version ') {
           agent {
                label "python-node"
                }
              steps {
                  sh "python3.9 --version"
                   }
                }

          stage('Building Docker Images') {
            agent {
                     label "python-node"
                   }
                steps {
                  sh "sudo chmod 666 /var/run/docker.sock"
                  sh "docker build -t ${REGISTRY}:${VERSION} ."
                     }
                 }

        stage('Push Docker Image To DockerHub') {
          // agent {
          //          label "python-node"
          //        }
              steps {
                   withCredentials([string(credentialsId: 'docker_password', variable: 'docker_pass')])  {
                   sh "docker login -u eagunuworld -p Aighegbe12345@ "
                   }
                 sh 'docker push ${REGISTRY}:${VERSION}'
                }
             }
      }
}
