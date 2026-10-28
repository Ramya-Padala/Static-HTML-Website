pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
           url: 'https://github.com/Ramya-Padala/Static-HTML-Website.git'
      }
    }
    stage('Docker Build') {
      steps {
        sh 'docker build -t html-nginx-app:latest .'
      }
    }
      stage('Docker test') {
        steps {
          sh 'docker ps -a'
        }
      }
        stage('Deploy') {
          steps {
            sh '''
            docker rm -f html-nginx-container 2>/dev/null || true
            docker run -d \
                        --name html-nginx-container \
                        --restart unless-stopped \
                        -p 8080:80 \
                        html-nginx-app:latest

            '''
          }
        }
          stage('Verify Website') {
            steps {
                sh '''
                    sleep 3
                    curl --fail http://127.0.0.1:8080
                '''
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully!'
            echo 'Access: http://YOUR_SERVER_IP:8080'
        }

        failure {
            echo 'Deployment failed. Check Console Output.'
        }
    }
}
