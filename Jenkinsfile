pipeline {
    agent any

    parameters {
        choice(
            name: 'TerraformAction',
            choices: ['apply', 'destroy'],
            description: 'Select Terraform action to execute: apply or destroy'
        )
    }

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    }

    stages {
        stage('Terraform Initialization') {
            steps {
                script {
                    echo 'Initializing Terraform...'
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Execution') {
            steps {
                script {
                    // Get user input
                    def terraformAction = params.TerraformAction

                    // Validate user input
                    if (terraformAction == 'apply') {
                        echo 'Executing Terraform apply...'
                        sh 'terraform apply -auto-approve'
                    } else if (terraformAction == 'destroy') {
                        echo 'Executing Terraform destroy...'
                        sh 'terraform destroy -auto-approve'
                    } else {
                        error "Invalid Terraform action selected: ${terraformAction}"
                    }
                }
            }
        }
    }
}

