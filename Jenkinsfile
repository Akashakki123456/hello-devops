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

        stage('Kubernetes Connection Test') {
            steps {
                withCredentials([
                    string(credentialsId: 'k8s-deploy-token', variable: 'K8S_TOKEN'),
                    string(credentialsId: 'k8s-ca-cert', variable: 'K8S_CA')
                ]) {
                    sh '''
                        set +x
                        CA_FILE=$(mktemp)
                        trap 'rm -f "$CA_FILE"' EXIT

                        printf '%s\\n' "$K8S_CA" > "$CA_FILE"
                        chmod 600 "$CA_FILE"

                        curl --silent --show-error \
                            --cacert "$CA_FILE" \
                            -H "Authorization: Bearer $K8S_TOKEN" \
                            https://192.168.102.152:6443/apis/apps/v1/namespaces/hello-devops/deployments \
                            -o /dev/null \
                            -w "HTTP Status: %{http_code}\\n"
                    '''
                }
            }
        }
    }
}
