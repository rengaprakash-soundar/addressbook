pipeline {
    
    agent { label 'slave' }
    stages {
	   
        stage ('Checkout') {
          steps {
            git 'https://github.com/rengaprakash-soundar/addressbook.git'
          }
        }
        stage('Build') {
            
             steps {
               
               sh '/opt/apache-maven-3.6.3/bin/mvn clean install'
                //junit '**/target/surefire-reports/TEST-*.xml'
		archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
        stage('Docker image') {
        
            steps {
                
                sh 'docker build -t rengaprakash123/addressbook$(git rev-parse HEAD):latest .'
            }
    }
        stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: '92dede17-31d7-4cbc-850a-3ea9ef02cc31', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push rengaprakash123/addressbook$(git rev-parse HEAD):latest'
        }
      }
    }
        stage('Docker Deploy') {
       
      steps {
          sh 'docker run -itd -P rengaprakash123/addressbook$(git rev-parse HEAD):latest'
        }
      }
    
    }
    
}
