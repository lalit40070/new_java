pipeline {
    agent any
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '10')
    }
  stages {
        stage('Compile and Clean') { 
            steps {

                sh "mvn clean compile"
            }
        }
        stage('deploy') { 
            steps {
                sh "mvn package"
            }
        }
    
    
    stage('Logging into AWS ECR') {
            steps {
                script {
                sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 226100319488.dkr.ecr.ap-south-1.amazonaws.com"
                }
                 
            }
        }
   
    stage('Building image') {
      steps{
        script {
             sh 'docker build -t dockerrepo:${BUILD_NUMBER} .'
        }
      }
    }
   stage('Docker Tag Image') {
      steps{
        script {
             sh 'docker tag dockerrepo:${BUILD_NUMBER}   226100319488.dkr.ecr.ap-south-1.amazonaws.com/dockerrepo:${BUILD_NUMBER} '
        }
      }
    }
      
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "docker push 226100319488.dkr.ecr.ap-south-1.amazonaws.com/dockerrepo:${BUILD_NUMBER}"
         }
        }
      }
  }
}
