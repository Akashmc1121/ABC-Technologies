pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        IMAGE_NAME      = "abc-technologies"
        CONTAINER_NAME  = "abc-technologies"
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

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

     stage('SonarQube Analysis') {
            steps {
                sh '''
                mvn sonar:sonar \
                  -Dsonar.projectKey=ABC-Technologies \
                  -Dsonar.projectName=ABC-Technologies \
                  -Dsonar.token=YOUR_ACTUAL_SONAR_TOKEN_HERE \
                  -Dsonar.host.url=http://your-sonarqube-server-url:9000
                '''
            }
        }

        stage('Package') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('SonarQube Analysis') {
            tools {
                jdk 'Jenkins-Configured-JDK17-Name' // Match the name from Manage Jenkins -> Tools
            }
            steps {
                sh '''
                mvn sonar:sonar \
                  -Dsonar.projectKey=ABC-Technologies \
                  -Dsonar.projectName=ABC-Technologies \
                  -Dsonar.token=YOUR_ACTUAL_SONAR_TOKEN \
                  -Dsonar.host.url=http://<YOUR-ACTUAL-SONAR-IP>:9000
                '''
            }
        }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }

        always {
            cleanWs()
        }
    }
}
