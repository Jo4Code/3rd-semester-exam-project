pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION = "us-east-1"
    }
    stages {
        stage("Create nginx-controller") {
            steps {
                script {
                    dir('nginxcontroller') {
                       sh "aws eks --region us-east-1 update-kubeconfig --name demo"
                        sh "terraform init"
                        sh "terraform apply -auto-approve"
                    }
                }
            }
        }

        stage("Create prometheusfile") {
            steps {
                script {
                    dir('prometheusfile') {
                        sh "terraform init"
                        sh "terraform apply -auto-approve"
                    }
                }
            }
        }

        stage("Deploy voting-application to EKS") {
            steps {
                script {
                    dir('voting-application') {
                        sh "kubectl apply -f voting-app.yaml"
                    }
                }
            }
        }

        stage("Deploy sockshopfile to EKS") {
            steps {
                script {
                    dir('sockshopfile') {
                        sh "kubectl apply -f complete-deployment.yaml"
                    }
                }
            }
        }

        stage("Deploy ingress to EKS") {
            steps {
                script {
                    dir('ingressfile') {
                        sh "terraform init"
                        sh "terraform apply -auto-approve"
                    }
                }
            }
        }
    }
}
