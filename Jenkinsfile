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
