pipeline {
    agent any
//this is just a comment to check a webhook

    stages {
        stage('Checkout Code') {
            steps {
                // This checks out the code from the GitHub repo
                // you will configure in the Jenkins job
                checkout scm
            }
        }
        
        stage('Build & Run Application') {
            steps {
                script {
                    echo 'Building and running application...'
                    // Use the docker-compose-ci.yml file
                    // --build forces it to rebuild if dependencies changed
                    // -d runs it in detached mode
                    sh 'docker compose -f docker-compose-ci.yml up --build -d'
                }
            }
        }
        
        stage('Check Status') {
            steps {
                // Wait a bit for services to start
                sh 'sleep 120'
                echo 'Checking container status...'
                sh 'docker ps'
            }
        }
    }
    
    // This post-build step will run no matter what,
    // to clean up the containers.
    post {
        always {
            echo 'Build complete. Tearing down CI environment...'
            sh 'docker compose -f docker-compose-ci.yml down --volumes'
        }
    }
}