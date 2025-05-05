pipeline {
    agent any
stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Install Newman') {
            steps {
                // Install Newman using npm
                sh 'npm install -g newman'
            }
        }

  stage('Run Postman Collection') {
            steps {
                script {
                    // Define the path to your Postman collection and environment file (if any)
                    def collectionPath = 'Monitor_Collection.postman_collection.json'

                    // Execute the Postman collection using Newman
                    sh """
                        newman run ${collectionPath} \
                        --reporters cli,junit \
                        --reporter-junit-export newman-report.xml
                    """
                }
            }
        }

        stage('Publish Results') {
            steps {
                // Archive the Newman JUnit report
                junit 'newman-report.xml'
            }
        }
    }

    post {
        success {
            echo 'Postman collection executed successfully!'
        }
        failure {
            echo 'Postman collection execution failed!'
        }
    }
}
