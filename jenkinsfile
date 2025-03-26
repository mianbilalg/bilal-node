pipeline {
    agent any  // Run on any available agent

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/mianbilalg/bilal-node.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building the project..."'
                sh 'docker build -t bilal-node .'  // اگر آپ کا پروجیکٹ Gradle پر ہے، ورنہ اپنی بلڈ کمانڈ دیں
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker tag bilal-node $DOCKER_USER/bilal-node:latest'
                    sh 'docker push $DOCKER_USER/bilal-node:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploying the project..."'
                sh 'docker-compose down && docker-compose up -d'

            }
        }
    }
}
