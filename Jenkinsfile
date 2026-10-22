
pipeline {
    agent any

    parameters {
        choice(
            name: 'BUILD_TYPE',
            choices: ['Development', 'Production'],
            description: 'Select the build type'
        )
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Thamaraiselvan-26/Assessment_10.git'
            }
        }

        stage('Parallel Build and Test') {
            parallel {
                stage('Validate HTML') {
                    steps {
                        bat 'findstr /C:"Smart Agriculture Dashboard" index.html'
                        echo 'HTML validation completed successfully.'
                    }
                }

                stage('Validate Dockerfile') {
                    steps {
                        bat 'docker --version'
                        bat 'docker build -t smart-agriculture-portal:latest .'
                    }
                }
            }
        }

        stage('Conditional Deployment Preparation') {
            when {
                expression {
                    params.BUILD_TYPE == 'Production'
                }
            }
            steps {
                echo 'Production build selected.'
                bat 'docker image inspect smart-agriculture-portal:latest'
            }
        }

        stage('Archive Build Files') {
            steps {
                archiveArtifacts artifacts: 'index.html,Dockerfile,deployment.yaml,Jenkinsfile',
                    fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Smart Agriculture Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the Jenkins Console Output.'
        }
    }
}
