pipeline {
    agent any
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev', 'test', 'preprod'], description: 'Select Environment')
    }
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout the branch based on the selected environment
                    echo "Checking out the branch for the ${params.ENVIRONMENT} environment."
                    checkout scm
                    // Ensure the branch is switched according to the ENVIRONMENT parameter
                    sh "git checkout ${params.ENVIRONMENT}"
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Replace `your-dockerhub-user` with your actual Docker Hub username
                    def imageTag = "malikdrote/image:${params.ENVIRONMENT}"
                    echo "Building Docker image for ${params.ENVIRONMENT} environment."
                    sh "docker build -t ${imageTag} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    def imageTag = "malikdrote/image:${params.ENVIRONMENT}"
                    echo "Pushing Docker image for ${params.ENVIRONMENT} environment."
                    sh "docker push ${imageTag}"
                }
            }
        }
        stage('Deploy Application') {
            steps {
                script {
                    def imageTag = "malikdrote/image:${params.ENVIRONMENT}"
                    echo "Deploying Docker container for ${params.ENVIRONMENT} environment."
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
