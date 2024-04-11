pipeline {
    agent any
    tools{
        maven 'maven3'
    }
    stages{
        stage('Build Maven'){
            steps{
                //checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Java-Techie-jt/devops-automation']]])
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Rajani45/devops-automation.git']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t rajaninandu/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u rajaninandu -p ${dockerhubpwd}'
                    
                }
                   sh 'docker push rajaninandu/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                sshagent(['k8s-ssh']) {
                 sh "scp -o StrictHostKeyChecking=no deploymentservice.yaml ubuntu@52.23.252.160:/home/ubuntu"
      script{
           try{
               sh "ssh  ubuntu@52.23.252.160 kubectl apply -f ."
           }
           catch(error){
               //sh " ubuntu@52.23.252.160 kubectl create -f ."
           }
       }
    }
  }
}
}
}
