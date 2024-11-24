pipeline {
    agent any

    parameters {
        string(name: 'MIN_SIZE', defaultValue: '1', description: 'Minimum instances in ASG')
        string(name: 'MAX_SIZE', defaultValue: '5', description: 'Maximum instances in ASG')
        string(name: 'DESIRED_CAPACITY', defaultValue: '1', description: 'Desired instances in ASG')
    }

    environment {
        AWS_CREDENTIALS = credentials('aws') // Replace 'aws-credentials' with your Jenkins credential ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Update Variables') {
            steps {
                bat """
                echo min_size = ${params.MIN_SIZE} > terraform.tfvars
                echo max_size = ${params.MAX_SIZE} >> terraform.tfvars
                echo desired_capacity = ${params.DESIRED_CAPACITY} >> terraform.tfvars
                """
            }
        }

        stage('Terraform Init') {
            steps {
                withEnv([
                    "AWS_ACCESS_KEY_ID=${AWS_CREDENTIALS_USR}",
                    "AWS_SECRET_ACCESS_KEY=${AWS_CREDENTIALS_PSW}"
                ]) {
                    bat 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                withEnv([
                    "AWS_ACCESS_KEY_ID=${AWS_CREDENTIALS_USR}",
                    "AWS_SECRET_ACCESS_KEY=${AWS_CREDENTIALS_PSW}"
                ]) {
                    bat 'terraform plan -var-file=terraform.tfvars'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                withEnv([
                    "AWS_ACCESS_KEY_ID=${AWS_CREDENTIALS_USR}",
                    "AWS_SECRET_ACCESS_KEY=${AWS_CREDENTIALS_PSW}"
                ]) {
                    bat 'terraform apply -auto-approve -var-file=terraform.tfvars'
                }
            }
        }
    }
}
