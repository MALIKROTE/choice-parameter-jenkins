pipeline {
    agent any
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev', 'test', 'preprod'], description: 'Select Environment')
    }
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // This will automatically check out the branch configured in the SCM section.
                    echo "Checked out the code from SCM."
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Replace `your-dockerhub-user` with your actual Docker Hub username
                    def imageTag = "malikdrote/image:${params.ENVIRONMENT}"
                    sh "docker build -t ${imageTag} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    def imageTag = "malikdrote/image:${params.ENVIRONMENT}"
                    sh "docker push ${imageTag}"
                }
            }
        }
        stage('Deploy Application') {
            steps {
                script {
                    def imageTag = "malikdrote/image:${params.ENVIRONMENT}"
                    // Pull and run the Docker container for the specified environment
                    sh """
                    docker pull ${imageTag}
                    docker run -d --name app-${params.ENVIRONMENT} -p 8085:80 ${imageTag}
                    """
                    echo "Deployed to ${params.ENVIRONMENT} environment."
                }
            }
        }
    }
}
