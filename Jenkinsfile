pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Ensure pipenv is installed and working
                sh 'pipenv install --dev'
            }
        }
        stage('Test') {
            steps {
                // Ensure pytest is installed and available
                sh 'pipenv run pytest'
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
                sh 'zip -r sbdl.zip lib'
            }
        }
        stage('Release') {
            when {
                branch 'release'
            }
            steps {
                // Ensure SCP works and paths are correct
                sh "scp -i /home/prashant/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-qa"
            }
        }
        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                // Ensure SCP works and paths are correct
                sh "scp -i /home/prashant/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-prod"
            }
        }
    }
}
