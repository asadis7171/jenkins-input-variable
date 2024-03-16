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
        AWS_ACCESS_KEY_ID = ''
        AWS_SECRET_ACCESS_KEY = ''
        AWS_DEFAULT_REGION = 'your-aws-region' // Add this line
    }

    stages {
        stage('Set Environment Variables') {
            steps {
                script {
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws_asad',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]]) {
                        // Credentials will be automatically injected into environment variables
                    }
                }
            }
        }
        
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

    post {
        always {
            echo 'Cleaning up...'
            deleteDir()
        }
    }
}

