pipeline {
    agent any

    environment {
        ACR_NAME = "myacrharni"
        IMAGE_NAME = "demo-app"
        AKS_RG = "aks"
        AKS_CLUSTER = "aksfirst"
    }

    stages {

        stage('Build Image') {
            steps {
                sh '''
                docker build -t $ACR_NAME.azurecr.io/$IMAGE_NAME:latest .
                '''
            }
        }

        stage('Push to ACR') {
            steps {
                sh '''
                az acr login --name $ACR_NAME
                docker push $ACR_NAME.azurecr.io/$IMAGE_NAME:latest
                '''
            }
        }

        stage('Deploy to AKS') {
            steps {
                sh '''
                az aks get-credentials --resource-group $AKS_RG --name $AKS_CLUSTER --overwrite-existing
                kubectl apply -f deployment.yaml
                '''
            }
        }
    }
}
