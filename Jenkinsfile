
pipeline {
    agent any

    tools {
        nodejs "Node1" // Ensure "Node1" is configured in Jenkins under Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install -g newman'
            }
        }

        stage('Run Postman Collection') {
            steps {
                script {
                    def collectionPath = 'Jenkins_Integration_Collection.postman_collection.json'
                    sh """
                        newman run ${collectionPath} --reporters cli,junit --reporter-junit-export newman-report.xml
                    """
                }
            }
        }

        stage('Publish Results') {
            steps {
                junit 'newman-report.xml'
            }
        }
    }

    post {
       
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
