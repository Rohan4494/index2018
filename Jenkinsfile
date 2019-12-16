pipeline {
    agent { label "access-portal-dev"
    }
    environment {
        
        DOCKER_IMAGE_NAME = "rohan4494/hello"
    }
    stages {
        stage('test') {
            steps {
                sh "sudo docker build . -t ${DOCKER_IMAGE_NAME}"
            }
        }
        stage('Push Docker Image') {
            
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            
            steps {
                input 'Deploy to Production?'
                milestone(1)
                sh "helm upgrade --install --wait --set image.repository=rohan4494/hello,image.tag=${env.BUILD_NUMBER} hello hello"
            }
        }
    }
}
