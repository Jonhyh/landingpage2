pipeline {
    agent any

    stages {
        stage('Verify') {
            steps {
                // This command lists the files to verify they were checked out correctly
                bat 'dir'
            }
        } 

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials',
                passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    bat 'docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'
                    bat 'docker push hojonathan/landingpage2'
                }
            }
        }

    }
}

