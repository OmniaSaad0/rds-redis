pipeline {
    agent {
        label "aws-slave"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        pwd
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin                        
                        docker build /home/ubuntu/jenkins/workspace/deploy-node-app/rds-redis -t omniasaad/node-jenkins:v2
                        docker push omniasaad/node-jenkins:v2
                    """
                }
            }
        }

        stage('deploy') {
            steps {
                sh """
                    docker run -d -p 3000:3000 --env-file /home/ubuntu/jenkins/workspace/deploy-node-app/rds-redis/.env omniasaad/node-jenkins:v2
                """
            }
        }
    }
}
