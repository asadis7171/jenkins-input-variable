pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = ''
        AWS_SECRET_ACCESS_KEY = ''
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
            steps {
                script {
                    // Prompt user for input during runtime
                    def userInput = input(
                        id: 'userInput',
                        message: 'Select Terraform action to execute: apply or destroy',
                        ok: 'Continue',
                        parameters: [choice(
                            name: 'TerraformAction',
                            choices: ['apply', 'destroy'],
                            description: 'Select Terraform action to execute'
                        )]
                    )

                    // Get user input and assign it to terraformAction variable
                    def terraformAction = userInput.TerraformAction?:''

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
