pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        EMAIL_RECIPIENTS = 'aryannn.mishra.2005@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh './deploy.sh'
            }
        }
    }

    post {
        success {
            emailext(
                subject: "SUCCESS: Job '${env.JOB_NAME} #${env.BUILD_NUMBER}'",
                body: """
                    <p>Build succeeded!</p>
                    <p>
                    Job: ${env.JOB_NAME}<br/>
                    Build: #${env.BUILD_NUMBER}<br/>
                    URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a>
                    </p>
                """,
                to: "${EMAIL_RECIPIENTS}",
                mimeType: 'text/html'
            )
        }

        failure {
            emailext(
                subject: "FAILURE: Job '${env.JOB_NAME} #${env.BUILD_NUMBER}'",
                body: """
                    <p>Build failed ❌</p>
                    <p>
                    Job: ${env.JOB_NAME}<br/>
                    Build: #${env.BUILD_NUMBER}<br/>
                    URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a>
                    </p>
                """,
                to: "${EMAIL_RECIPIENTS}",
                mimeType: 'text/html'
            )
        }

        always {
            echo 'Pipeline finished.'
        }
    }
}
