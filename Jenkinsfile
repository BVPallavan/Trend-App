pipeline {
   agent any
   stages {
     stage('Clone') {
	steps {	 git branch: 'main', url: 'https://github.com/BVPallavan/Trend-App.git' }
     }
     stage('Build Docker') {
       steps { sh 'docker build -t trend-app:latest .' }
     }
     stage('Push DockerHub') {
       steps {
         withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
           sh 'docker login -u $USER -p $PASS'
           sh 'docker tag trend-app:latest bvpallavan/trend-app:latest'
           sh 'docker push bvpallavan/trend-app:latest'
         }
       }
     }
     stage('Deploy to EKS') {
       steps {
         sh 'kubectl apply -f deployment.yaml'
         sh 'kubectl apply -f service.yaml'
       }
     }
   }
 }
