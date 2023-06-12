pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/shruthi117/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t shruthi117/kubernetes:$BUILD_NUMBER .'
                }
            }
        }
        stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'shruthi117-dockerhub', variable: 'dockerhub')]) {
                    sh 'docker login -u shruthi117 -p ${dockerhub}'
                        
                    }
                    sh 'docker push shruthi117/kubernetes:$BUILD_NUMBER'
                }
            }
        }
       
        stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubernetes')
                 
                }
            }
        }
    
    }    
}
