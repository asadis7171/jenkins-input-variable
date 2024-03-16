pipeline {
    agent any

    parameters {
        choice(
            name: 'TerraformAction',
            choices: ['apply', 'destroy'],
            description: 'Select Terraform action to execute: apply or destroy'
        )
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
                        withCredentials([[
                            $class: 'AmazonWebServicesCredentialsBinding',
                            credentialsId: 'aws_asad',
                            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                        ]]) {
                            sh 'terraform apply -auto-approve'
                        }
                    } else if (terraformAction == 'destroy') {
                        echo 'Executing Terraform destroy...'
                        withCredentials([[
                            $class: 'AmazonWebServicesCredentialsBinding',
                            credentialsId: 'aws_asad',
                            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                        ]]) {
                            sh 'terraform destroy -auto-approve'
                        }
                    } else {
                        error "Invalid Terraform action selected: ${terraformAction}"
                    }
                }
            }
        }
    }
}

