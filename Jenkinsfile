pipeline{
    agent any
    tools{
        maven 'maven'
        // maven 'maven_3_9_6'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/chiehminchung/devops-automation']])
                sh 'mvn clean install'

            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t chiehmin/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u chiehmin -p${dockerhubpwd}'
                        sh 'docker push chiehmin/devops-integration'
                    }
                }
            }
        }
    }
}