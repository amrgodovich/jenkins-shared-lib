@Library('jenkins-shared-lib') _

pipeline {
    agent { label 'Maven' }
    tools { maven 'Maven-3.8.9' }

    environment {
        IMG = "amrgodovich/shared-lib-jenkins-libb"
        TAG = "${BUILD_NUMBER}"
    }

    stages {
        
        stage('Test') {
            steps {
                echo "Testing docker build"
                dockerBuild('test-image', 'latest')
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/amrgodovich/neww'
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Docker Build') {
            steps {
                dockerBuild(IMG, TAG)
            }
        }

        stage('Docker Login') {
            steps {
                dockerLogin("amrgodovich", "dockerhub-pass")
            }
        }

        stage('Docker Push') {
            steps {
                dockerPush(IMG, TAG)
            }
        }

        stage('Deploy') {
            steps {
                dockerDeploy(IMG, TAG)
            }
        }
    }
}
