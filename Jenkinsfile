pipeline { 
    agent any 
    stages {
        stage('Stage01 - Clone the code from GitHub repo') {
            steps {
                git branch: 'main', url: 'https://github.com/amitkumar0441/mytwotierapplicationmanagebyjenkins.git'
            }
        }
        
        stage('Stage02-Move Code to Amit Directory Using cp') {
            steps {
                script {
                    // Use find and cp to copy files, excluding .git and ..
                    sh '''
                    find . -maxdepth 1 -mindepth 1 ! -name .git ! -name .. -exec cp -r {} /home/amit/cicdflaskproject/ \;
                    '''
                }
            }
        }
        stage('Stage03 - Build the Docker image from Dockerfile') {
            steps {
                sh 'docker build -t amitkumar0441/myflaskapp:latest .'
            }
        }
        stage('Stage04 - Push Docker Image to Docker Hub') {
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
        stage('Stage05 - Deploy the applications with the use of docker-compose') {
            steps {
                sh 'docker-compose down && docker-compose up -d '
            }
        }
    }
}
