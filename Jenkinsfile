pipeline {
    agent any

    stages {

        stage('Cloning Git') {
            steps {
                git branch: 'master', url: 'https://github.com/HafidElmoudden/devops-tp'
            }
        }

        stage('Building image') {
            steps {
                bat 'docker build -t hafidelmoudden/devops-tp:latest .'
            }
        }

        stage('Test image') {
            steps {
                bat 'docker run --rm hafidelmoudden/devops-tp:latest cat /usr/share/nginx/html/index.html'
            }
        }

        stage('Publish image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                        docker push hafidelmoudden/devops-tp:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy image') {
            steps {
                bat '''
                    docker stop devops-tp-container || exit 0
                    docker rm devops-tp-container || exit 0
                    docker run -d -p 8080:80 --name devops-tp-container hafidelmoudden/devops-tp:latest
                '''
            }
        }
    }
}
