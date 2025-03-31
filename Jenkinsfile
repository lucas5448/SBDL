pipeline {
    agent any

    stages {
        stage('Install Pipenv') {
            steps {
                // Ensure pipenv is installed
                bat 'pip install pipenv'
            }
        }
        stage('Build') {
            steps {
                // Install dependencies using pipenv
                bat 'pipenv install --dev'
            }
        }
        stage('Test') {
            steps {
                // Run tests using pipenv
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
                // Package the project into a zip file
                bat 'zip -r sbdl.zip lib'
            }
        }
        stage('Release') {
            when {
                branch 'release'
            }
            steps {
                // Release the packaged files to the QA environment
                bat "scp -i /home/prashant/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-qa"
            }
        }
        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                // Deploy the packaged files to the production environment
                bat "scp -i /home/prashant/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-prod"
            }
        }
    }
}
