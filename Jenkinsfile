node {
    def app
    

		// Mark the code checkout 'stage'....
		stage('Checkout from Github') {
			checkout scm
		}


		// Build and Deploy to ACR 'stage'... 
		stage('Build and Push to Azure Container Registry') {
			app = docker.build('cptdockerregistry.azurecr.io/event-service')
			docker.withRegistry('https://cptdockerregistry.azurecr.io', 'acr-cred') {
				app.push("${env.BUILD_NUMBER}")
				app.push('latest')
			}
		}

		// Pull, Run, and Test on ACS 'stage'... 
		stage('ACS Docker Pull and Run') {
	   		app = docker.image('cptdockerregistry.azurecr.io/event-service:latest')
	   		docker.withRegistry('https://cptdockerregistry.azurecr.io', 'acr-cred') {
				app.pull()
				//app.run('--name event-service -p 8082:8082')
                                sh '/usr/local/bin/docker-compose down'
                                sh '/usr/local/bin/docker-compose up -d'
			}
		}

   
}
