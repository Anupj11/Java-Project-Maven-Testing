pipeline {
    agent any

    environment {
        PROJECT_NAME = "JtSpringProject"
        GITHUB_REPO_URL = "https://github.com/Anupj11/Java-Project-Maven-Testing.git"
        DOCKER_IMAGE = "jt-spring-tomcat-ui"
        CONTAINER_NAME = "jt-ui"
    }

    tools {
        maven 'Maven-3.8.6'
        jdk 'JDK-11'
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning source code..."
                git url: "${GITHUB_REPO_URL}", branch: 'master'
            }
        }

        stage('Build WAR') {
            steps {
                echo "Building WAR file..."
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Verify Artifact') {
            steps {
                echo "Checking WAR file..."
                sh 'ls -lh target'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Tomcat Docker image..."
                sh '''
                docker build -f Dockerfile.tomcat -t $DOCKER_IMAGE .
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying application container..."
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d -p 8080:8080 --name $CONTAINER_NAME $DOCKER_IMAGE
                '''
            }
        }
    }

    post {
        success {
            echo " CI/CD Pipeline completed successfully"
        }
        failure {
            echo " CI/CD Pipeline failed"
        }
        always {
            cleanWs()
        }
    }
}
