pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'malu12345/mywebapp'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', credentialsId: 'gitid', url: 'https://github.com/aryapillai/ci-cd-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerid', url: 'https://index.docker.io/v1/']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy on EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@13.235.74.87 <<EOF
if [ \$(docker ps -aq -f name=webapp) ]; then
  stage('Deploy on EC2') {
    steps {
        sshagent(['ec2-ssh-key']) {
            sh '''
            ssh -o StrictHostKeyChecking=no ec2-user@13.235.74.87 <<EOF
if [ $(docker ps -aq -f name=webapp) ]; then
    echo "Stopping and removing existing webapp container..."
    docker stop webapp
    docker rm webapp
fi
docker pull $DOCKER_IMAGE
docker run -d -p 80:80 --name webapp $DOCKER_IMAGE
EOF
            '''
        }
    }
}
  echo "Stopping and removing existing webapp container..."
    docker stop webapp
    docker rm webapp
fi
                        docker pull $DOCKER_IMAGE
                        docker run -d -p 80:80 --name webapp $DOCKER_IMAGE
                    EOF
                    '''
                }
            }
        }
    }
}

