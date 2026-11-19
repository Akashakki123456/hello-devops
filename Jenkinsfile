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
                sh 'docker tag hello-devops:v2 akashakki123456/hello-devops:v2'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker push akashakki123456/hello-devops:v2
                        docker logout
                    '''
                }
            }
        }
    }
}
