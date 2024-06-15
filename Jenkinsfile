pipeline {

    agent any

    environment {
        registry = "mmonteseoi/contenedor-lanzadado"
        registryCredentials = "dockerhub"
        project = "Practica-Jenkins"
        projectVersion = "1.0"
        repository = "https://github.com/mmontesEOI/PracticaDevOps.git"
        repositoryCredentials = "github"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout code') {
            steps {
                script {
                    git branch: 'main',
                        credentialsId: repositoryCredentials,
                        url: repository
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    try {
                        sh 'docker run --name $project $registry'
                    } finally {
                        sh 'docker rm $project'
                    }

                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('',registryCredentials) {
                        dockerImage.push()
                    }
                }
            }
        }
    }

    post {
        failure {
            echo 'El pipeline ha fallado'
        }
    }

}
