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
              steps {
                   withCredentials([string(credentialsId: 'docker_password', variable: 'docker_pass')])  {
                   sh "docker login -u eagunuworld -p Aighegbe12345@ "
                   }
                 sh 'docker push ${REGISTRY}:${VERSION}'
                }
             }
    stage('stop previous containers') {
            steps {
               sh 'docker ps -f name=framed -q | xargs --no-run-if-empty docker container stop'
               sh 'docker container ls -a -fname=framed  -q | xargs -r docker container rm'
            }
          }

    stage('Docker Run') {
        steps{
            script {
              sh 'docker run -d -p 8085:5000 --rm --name framed ${REGISTRY}:${VERSION}'
               }
             }
         }
    stage('Deleting Old versions ') {
        agent {
              label "python-node"
                  }
            steps {
                sh "docker rmi -f ${REGISTRY}:${VERSION}"
              }
        }
   }
}
