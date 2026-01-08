pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo "Building the application..."
                // Add your build commands here
            }
        }
        
        stage('Test') {
            steps {
                echo "Testing the application..."
                // Add your test commands here
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'  // Only deploy from main branch
            }
            steps {
                echo "Deploying the application..."
                // Add your deployment commands here
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
