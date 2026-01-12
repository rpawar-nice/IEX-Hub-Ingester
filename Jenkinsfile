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

        stage('Deploy') {
            when {
                branch 'dev'
            }
            steps {
                echo "Deploying application from main branch..."
                sh '''
                    scp ${PACKAGE_NAME} ${SERVER_USER}@${SERVER_HOST}:/tmp/
                    ssh ${SERVER_USER}@${SERVER_HOST} "
                        mkdir -p ${DEPLOY_PATH} &&
                        cd ${DEPLOY_PATH} &&
                        tar -xzf /tmp/${PACKAGE_NAME} &&
                        pkill -f ${APP_NAME} || true &&
                        nohup java -jar ${DEPLOY_PATH}/${BUILD_DIR}/*.jar > app.log 2>&1 &
                    "
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
