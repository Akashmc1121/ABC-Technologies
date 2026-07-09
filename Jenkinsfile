pipeline {

    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK17'
    }

    environment {
        IMAGE_NAME = "abc-technologies"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    mvn sonar:sonar \
                    -Dsonar.projectKey=ABC-Technologies \
                    -Dsonar.projectName=ABC-Technologies
                    '''
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t abc-technologies .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                docker rm -f abc-technologies || true
                docker run -d \
                --name abc-technologies \
                -p 8082:8080 \
                abc-technologies
                '''
            }
        }

    }
}
