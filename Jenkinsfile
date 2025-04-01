pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout the code from the Git repository
                checkout scm
            }
        }

        stage('Install Pipenv') {
            steps {
                // Install pipenv to ensure it's available
                bat 'pip install pipenv'
            }
        }

        stage('Build') {
            steps {
                // Ensure pipenv is using the correct Python version
                bat 'pipenv --python 3.11 install --dev'
            }
        }

        stage('Test') {
            steps {
                // Ensure pytest is installed and available
                bat 'pipenv run pytest'
            }
        }

        stage('Package') {
            when {
                anyOf {
                    branch "master"
                    branch 'release'
                }
            }
            steps {
                // Ensure packaging works and necessary files are included
                bat 'zip -r sbdl.zip lib'
            }
        }

        stage('Release') {
            when {
                branch 'release'
            }
            steps {
                // Ensure SCP works and paths are correct
                bat "scp -i /home/prashant/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-qa"
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                // Ensure SCP works and paths are correct
                bat "scp -i /home/prashant/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-prod"
            }
        }
    }
}
