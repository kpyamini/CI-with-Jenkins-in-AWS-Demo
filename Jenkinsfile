pipeline {
    agent any 
    environment {
        PROJECT_ID = 'devops-yamini'
        CLUSTER_NAME = 'kube-demo'
        LOCATION = 'us-central1-a'
        CREDENTIALS_ID = 'kuberneteslogin'
    }
    stages {
    	   	stage ("Build") {
			steps {
		                sh 'mvn package'}}
        	stage ("Build image") {
			steps {
                			script {myapp = docker.build("223011/k8s:${env.BUILD_ID}")}}}
        	stage ("Push image") {
			steps {script {docker.withRegistry('https://registry.hub.docker.com', 'docker') {myapp.push("${env.BUILD_ID}"
)}}} }        
        	stage('Deploy to Google Kubernetes') {
            steps{
			    echo "Deployment started"
				sh 'ls -ltr'
				sh 'pwd'
                sh "sed -i 's/tagversion/${env.BUILD_ID}/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
				echo "Deployment Finished"
            }
        }
    }}
