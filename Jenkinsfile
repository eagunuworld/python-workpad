pipeline {
    //agent none
    agent{
          label "pythonAgent"
       }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'buld-Test-demo']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/eagunuworld/python-workpad.git']]])
            }
        }
        stage('Build') {
            steps {
                git branch: 'buld-Test-demo', url: 'https://github.com/eagunuworld/python-workpad.git'
                sh 'python3 ops.py'
            }
        }
        stage('Test') {
            steps {
                sh 'python3 -m pytest'
            }
        }
    }
}
