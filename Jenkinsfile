pipeline {
    agent any
    
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        AWS_DEFAULT_REGION = 'us-east-1'
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

        stage('Terraform Execution') {
            steps {
                script {
                    // Terraform initialization
                    echo 'Initializing Terraform...'
                    sh 'terraform init'
                }
            }
        }
        
        stage('Prompt for Terraform Action') {
            input {
                message "Select Terraform action to execute: apply or destroy"
                ok "Continue"
                parameters {
                    choice(
                        name: 'TerraformAction',
                        choices: ['apply', 'destroy'],
                        description: 'Select Terraform action to execute'
                    )
                }
            }
            steps {
                script {
                    // Get user input
                    def terraformAction = params.TerraformAction

                    // Validate user input
                    if (terraformAction == 'apply' || terraformAction == 'destroy') {
                        echo "Executing Terraform $terraformAction..."
                        sh "terraform $terraformAction -auto-approve"
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
