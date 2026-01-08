pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
    }

    tools {
        terraform 'terraform'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-org/iac-terraform-repo.git'
            }
        }

        stage('Terraform Init') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-creds']]) {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-creds']]) {
                    sh 'terraform plan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-creds']]) {
                    sh 'terraform apply -auto-approve'
                }
            }
        }
    }

    post {
        success {
            echo 'Infrastructure created successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
