pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
        DOCKER_USERNAME = 'nesrine419'
        KUBE_CONFIG_PATH = '/home/ubuntu/.kube/config'  // Chemin vers le fichier kubeconfig
        K8S_CONFIGS_PATH = '/home/ubuntu/k8s-configs'  // Chemin vers le répertoire des fichiers YAML
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Authentication') {
            steps {
                dir('Authentifcation_Verif_Email') {
                    // Nettoyer le projet et l'installer
                    sh 'mvn clean install -DskipTests=true'
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                withDockerRegistry(credentialsId: 'docker-nesrine', url: "") {
                    // Construire l'image Docker pour le backend
                    sh 'docker build -t nesrine419/backend -f ./Dockerfile2 Authentifcation_Verif_Email'
                    sh 'docker push nesrine419/backend'

                     Construire l'image Docker pour le frontend
                     sh 'docker build --no-cache -t nesrine419/frontend -f ./Dockerfile1 frontend-application'
                     sh 'docker push nesrine419/frontend'
                }
            }
        }

       stage('Deploy to Kubernetes') {
    steps {
        script {
            // Exécuter les commandes directement sur le serveur master via SSH
            sh "ssh -o StrictHostKeyChecking=no ubuntu@15.237.184.13 'kubectl apply -f /home/ubuntu/k8s-configs/backend-deployment.yaml'"
            sh "ssh -o StrictHostKeyChecking=no ubuntu@15.237.184.13 'kubectl apply -f /home/ubuntu/k8s-configs/frontend-deployment.yaml'"
            sh "ssh -o StrictHostKeyChecking=no ubuntu@15.237.184.13 'kubectl apply -f /home/ubuntu/k8s-configs/mysql-deployment.yaml'"
            sh "ssh -o StrictHostKeyChecking=no ubuntu@15.237.184.13 'kubectl apply -f /home/ubuntu/k8s-configs/ingress.yaml'"
        }
    }
}

stage('Display Kubernetes Resources') {
    steps {
        script {
            // Afficher les Pods
            sh "ssh -o StrictHostKeyChecking=no ubuntu@15.237.184.13 'kubectl get pods'"

            // Afficher les Deployments
            sh "ssh -o StrictHostKeyChecking=no ubuntu@15.237.184.13 'kubectl get deployments'"

            // Afficher les Ingress
            sh "ssh -o StrictHostKeyChecking=no ubuntu@15.237.184.13 'kubectl get ingress'"

            // Afficher les Services
            sh "ssh -o StrictHostKeyChecking=no ubuntu@15.237.184.13 'kubectl get services'"
        }
    }
}

       
    }
}
