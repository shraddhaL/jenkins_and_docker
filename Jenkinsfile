pipeline {
    agent any
    environment {
        containerName = ""
        container_version = "1.0.0.${BUILD_ID}"
        dockerTag = "${containerName}:${container_version}"
    }
    stages{
        
        stage('Clone repository') {
			   steps {	       
				 checkout scm }
        
			   }
        
        stage('Build') {
			   steps {	       
				 bat 'rm target/roshambo.war'
                 bat  'rm -rf target/roshambo' 
               
               }
        
			   }
        
        
        stage ('Build Container') {
            steps {
                sh 'docker build -f "Dockerfile" --no-cache -t ${dockerTag} .'
                //sh 'docker build -f "Dockerfile" -t ${dockerTag} .'
                  }
             }
        stage('Docker Push') {
            // agent any
            steps {
             withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
             sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
             sh 'docker push ${dockerTag}'
             }
        }
      }
        stage('Docker Cleanup') {
              steps {
                sh "docker images ${dockerTag} -q | tee ./xxx"
            sh 'docker rmi `cat ./xxx` --force ||exit 0'
            }
        }
        
    }
} 
         
