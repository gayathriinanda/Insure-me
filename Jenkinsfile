pipeline {
    
     agent any

    stages {
        stage('Code-Checkout') {
            steps {
               checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/gayathriinanda/Insure-me.git']])
            }
        }
        
        stage('Code-Build') {
            steps {
               sh 'mvn clean package'
            }
        }
        
        stage('Containerize the application'){
            steps { 
               echo 'Creating Docker image'
               sh "docker build -t gayathri/insurance:latest ."
            }
        }
        
        stage('Docker Push') {
    	agent any
          steps {
       	withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            	sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh 'docker push gayathri/insurance:latest'
        }
      }
    }
    
      
     stage('Code-Deploy') {
        steps {
           ansiblePlaybook credentialsId: 'ansible', installation: 'ansible', playbook: 'playbook.yml', vaultTmpPath: ''       
        }
      }
   }
}
