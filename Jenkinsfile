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
                bat 'docker run --rm hafidelmoudden/devops-tp:latest powershell -Command "Get-Content /usr/share/nginx/html/index.html"'
            }
        }

        stage('Publish Image') {
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
    }
}
