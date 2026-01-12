pipeline {
    agent {
    label 'dockervm'
    }

    environment {
        // Git Configuration
        GIT_REPO_URL = 'https://github.com/rpawar-nice/IEX-Hub-Ingester.git'
        GIT_CREDENTIALS_ID = 'jenkinswfmgituser'
        
        // Build Configuration
        APP_NAME = 'IEX-Hub-Ingester'
        ARTIFACT_NAME = "${APP_NAME}-${env.BUILD_NUMBER}. jar"
    }
  
    tools {
        maven 'Maven 3.9.9'
        jdk 'Azul 21.36.18 JDK 21.0.4 x64 (Linux archive extract)'
    }

  
    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out branch: ${params.GIT_BRANCH}"
                git branch: "${params.GIT_BRANCH}",
                    url: "${env.REPO_URL}"
                checkout scm
            }
            
        }

        stage('Initialize') {
            steps {
                echo "=========================================="
                echo "Initializing Build Environment"
                echo "=========================================="
                script {
                    sh '''
                        echo "Java Version:"
                        java -version
                        
                        echo ""
                        echo "Maven Version:"
                        mvn -version
                    '''    
                }
            }
        }
        
        stage('Build') {
            steps {
                echo "Building Java application..."
                sh '''
                    mvn clean compile
                '''
            }
        }
      
        stage('Test') {
            steps {
                echo "Running tests..."
                sh '''
                    mvn test
                '''
            }
        }
      
        stage('Package') {
            steps {
                echo "Packaging JAR..."
                sh '''
                    mvn package -DskipTests
                    mkdir -p ${BUILD_DIR}
                    cp target/*.jar ${BUILD_DIR}/
                    tar -czf ${PACKAGE_NAME} ${BUILD_DIR}
                '''
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
