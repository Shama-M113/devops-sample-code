pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Creating virtual environment and installing dependencies..."
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    if [ -f requirements.txt ]; then
                        pip install -r requirements.txt
                    else
                        pip install flask
                    fi
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh '''
                    . venv/bin/activate
                    python3 -m unittest discover -s .
                '''
            }
        }

        stage('Deploy') {
            when { expression { currentBuild.currentResult == "SUCCESS" } }
            steps {
                echo "Deploying application..."
            }
        }

        stage('Run Application') {
            when { expression { currentBuild.currentResult == "SUCCESS" } }
            steps {
                echo "Running Flask app..."
                sh '''
                    . venv/bin/activate
                    python3 app.py &
                '''
            }
        }

        stage('Test Application') {
            when { expression { currentBuild.currentResult == "SUCCESS" } }
            steps {
                echo "Testing running application..."
            }
        }
    }
    
    post {
        failure {
            echo "Pipeline failed. Check the logs for more details."
        }
        success {
            echo "Pipeline completed successfully."
        }
    }
}




