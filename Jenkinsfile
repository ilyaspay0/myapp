pipeline {
    agent any

    environment {
        SONARQUBE = credentials(sqa_38ad4959194a870f39f6d6a3d552bb637118c150) // ton token stocké dans Jenkins
        DOCKER_IMAGE = "myapp:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ilyaspay0/myapp.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonarServer') {
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=myapp \
                        -Dsonar.sources=./src \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONARQUBE
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run --rm myapp:latest'
            }
        }

        stage('Post Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, test et analyse réussis !'
        }
        failure {
            echo '❌ Une erreur est survenue pendant le pipeline.'
        }
    }
}
