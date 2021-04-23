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
	stage('Delpoying the App on Azure Kubernetes Service') {            
    app = docker.image('cptdockerregistry.azurecr.io/event-service:latest')            
    withDockerRegistry([credentialsId: 'acr-cred', url: 'https://cptdockerregistry.azurecr.io']) {            
    app.pull()            
    sh "kubectl create -f ."            
    }       
   }  
}
