pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build --network host -t saeednokarizi/app:v1 .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm saeednokarizi/app:v1 pytest'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker push saeednokarizi/app:v1'
                sh '''
                ssh -o StrictHostKeyChecking=no -i your-aws-key.pem ec2-user@your-aws-instance-public-ip << EOF
                docker pull saeednokarizi/app:v1
                docker stop $(docker ps -q)
                docker run -d -p 80:5000 saeednokarizi/app:v1
                EOF
                '''
            }
        }
    }
}

