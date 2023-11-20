pipeline {
    agent any

    environment {
        // Define any environment variables needed for the pipeline
        GIT_URL = 'https://github.com/harishhd6/docker-jenkins-integration-sample.git'
        GIT_CREDENTIALS_ID = 'git-credentials-id'
        DOCKER_REGISTRY = 'docker.io/harishhd6/docker-publish-jenkn'
        DOCKER_IMAGE_NAME = 'docker-jenkins'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the specified Git repository and branch
                script {
                    checkout([$class: 'GitSCM', 
                              branches: [[name: 'master']], 
                              doGenerateSubmoduleConfigurations: false, 
                              extensions: [[$class: 'CleanBeforeCheckout'], 
                                           [$class: 'CloneOption', depth: 1, noTags: true, shallow: true, reference: '']], 
                              submoduleCfg: [], 
                              userRemoteConfigs: [[url: GIT_URL, credentialsId: GIT_CREDENTIALS_ID]]])
                }
            }
        }

        stage('Build with Maven') {
            steps {
                // Build the project using Maven
                script {
                     sh '/opt/maven/bin/mvn clean install'
                }
            }
        }
        stage('Build and publish Docker image') {
            steps {
                // Build and publish Docker image
                script {
                    sh "docker build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME} ."

                }
            }
        }

    }
}

