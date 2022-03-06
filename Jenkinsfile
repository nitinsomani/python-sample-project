pipeline{
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-credentials')
	}
    stages{
        stage('scm stage'){
           steps{
                checkout scm
            }
        }
        stage('build stage'){
            steps{
                sh 'docker build -t nitinsomani/pythonapp:$env.BUILD_TIMESTAMP .'
            }
        }
        stage('push stage'){
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push nitinsomani/pythonapp:$env.BUILD_TIMESTAMP'
            }
        }
        stage('deploy stage'){
            steps{
                sh 'docker container run -itd -p 8081:8081 nitinsomani/pythonapp:$env.BUILD_TIMESTAMP'
            }
        }
    }
    post{
        always{
            sh 'docker logout'
        }
    }
}