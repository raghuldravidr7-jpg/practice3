pipeline {
    agent any
    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'staging', 'prod'],
            description: 'Target environment for the pipeline execution.'
        )
    }
    stages {
        stage('Checkout') {
            steps {
                echo "Checking out source code from repository..."
            }
        }
        stage('Build') {
            steps {
                echo "Running compile check on app.py using py_compile..."
                sh 'python3 -m py_compile app.py'
            }
        }
        stage('Deploy') {
            stages {
                stage('Approval Gate') {
                    steps {
                        script {
                            input message: "Approve deployment to ${params.ENVIRONMENT}?", ok: 'Go'
                        }
                    }
                }
                stage('Execute Application') {
                    steps {
                        echo "Deployment approved! Running ${params.ENVIRONMENT} configuration..."
                        sh 'python3 app.py'
                    }
                }
            }
        }
    }
}
