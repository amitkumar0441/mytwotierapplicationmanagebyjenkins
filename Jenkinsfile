pipeline { 
    agent any 
    stages {
        stage('Stage01 - Clone the code from GitHub repo') {
            steps {
                git branch: 'main', url: 'https://github.com/amitkumar0441/mytwotierapplicationmanagebyjenkins.git'
            }
        }
        stage('Stage02 - Build the Docker image from Dockerfile') {
            steps {
                sh 'docker build -t amitkumar0441/myflaskapp:latest .'
            }
        }
        stage('Stage03 - Push Docker Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        docker login -u "$DOCKER_USER" -p "$DOCKER_PASS"
                        docker push amitkumar0441/myflaskapp:latest
                        docker logout
                    '''
                }
            }
        }
        stage('Stage04 - Deploy the applications with the use of docker-compose') {
            steps {
                sh 'docker-compose down && docker-compose up -d '
            }
        }
    }
}
