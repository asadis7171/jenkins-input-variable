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
                        withAWS(credentials: 'aws_asad', region: 'us-west-2') {
                            sh 'terraform apply -auto-approve'
                        }
                    } else if (terraformAction == 'destroy') {
                        echo 'Executing Terraform destroy...'
                        withAWS(credentials: 'aws_asad', region: 'us-west-2') {
                            sh 'terraform destroy -auto-approve'
                        }
                    } else {
                        error "Invalid Terraform action selected: ${terraformAction}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            deleteDir()
        }
    }
}

