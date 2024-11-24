pipeline {
    agent any

    parameters {
        string(name: 'MIN_SIZE', defaultValue: '1', description: 'Minimum instances in ASG')
        string(name: 'MAX_SIZE', defaultValue: '5', description: 'Maximum instances in ASG')
        string(name: 'DESIRED_CAPACITY', defaultValue: '1', description: 'Desired instances in ASG')
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Update Variables') {
            steps {
                script {
                    // Update Terraform variables dynamically
                    sh """
                    echo 'min_size = ${params.MIN_SIZE}' > terraform.tfvars
                    echo 'max_size = ${params.MAX_SIZE}' >> terraform.tfvars
                    echo 'desired_capacity = ${params.DESIRED_CAPACITY}' >> terraform.tfvars
                    """
                }
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -var-file=terraform.tfvars'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve -var-file=terraform.tfvars'
            }
        }
    }
}
