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
        
           sh 'docker login -u $USER -p $PASS'
           sh 'docker tag trend-app:latest bvpallavan/trend-app:latest'
           sh 'docker push bvpallavan/trend-app:latest'
       
       }
     }
     stage('Deploy to EKS') {
       steps {
		   script {
               withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS']]) {
				sh 'aws eks update-kubeconfig --region ap-south-1 --name project2-cluster'
         		sh 'kubectl apply -f deployment.yaml'
         		sh 'kubectl apply -f service.yaml'
			   }
		   }
       }
     }
   }
 }
