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
}
