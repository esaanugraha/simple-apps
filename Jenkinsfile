pipeline {
    agent {
        node {
            label "ubuntu-docker"
        }
    }

    environment {
        // Mengatur variabel lingkungan
        GIT_REPO = 'https://github.com/esaanugraha/simple-apps.git'
    }

    tools {nodejs "nodejs 18.16.0"}

    stages {
        stage('CheckOut SCM') {
            steps {
                git branch: 'main', url: ("${env.GIT_REPO}")
            }
        }

        stage('BUILD') {
            steps {
                sh '''cd app
                npm install'''
            }
        }

       stage('TEST') {
            steps {
                sh '''cd app
                npm test'''
            }
       }
        
        stage('Code Review') {
            steps {
                sh '''cd app
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://10.0.0.57:9000 \
                -Dsonar.login=squ_81ef046f96edafd2c86d259a3ca4a1a776320bc0'''
            }
        }
        stage('DEPLOY-COMPOSE') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }

        stage('PUSH') {
            steps {
                sh '''
                docker push esanugraha/simple-esa:latest
                '''
            }
        }
        
    }

    post{
        always {
            echo "I will always Hello again ESA !"
        }
        success {
            echo "Yey, Success"
        }
        failure {
            echo "Oh no, Failure"
        }
        cleanup {
            echo "Don't care success or error"
        }
    }    
}