pipeline {
    agent any

    stages {

        stage('Checkout Verification') {
            steps {
                echo 'Jenkins is connected to GitHub!'
                sh 'pwd'
                sh 'ls -la'
                sh 'cat index.html'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t hello-devops:v2 .'
            }
        }
    }
}
