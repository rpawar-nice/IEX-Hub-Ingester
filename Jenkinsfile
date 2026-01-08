pipeline {
    agent dockervm
    
    stages {
        stage('Build') {
            steps {
                echo "Building branch: ${env.BRANCH_NAME}"
                // Add your build commands here
            }
        }
        
        stage('Test') {
            steps {
                echo "Testing branch: ${env.BRANCH_NAME}"
                // Add your test commands here
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'  // Only deploy from main branch
            }
            steps {
                echo "Deploying from main branch"
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
