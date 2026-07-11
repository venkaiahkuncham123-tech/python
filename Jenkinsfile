pipeline {
    agent any
    environment {
        SONAR_HOME = tool 'venkaiah-sonar-scanner'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/venkaiahkuncham123-tech/python.git'
            }
        }
        
        stage('Installing Dependencies') {
            steps {
               dir('src') {
                   script {
                       // 1. Install system dependencies (Make sure the Jenkins user has sudo permissions, or pre-install this on the agent)
                       sh "sudo apt-get update && sudo apt-get install python3-venv -y" 
                       
                       // 2. Create the virtual environment
                       sh "python3 -m venv venv"
                       
                       // 3. Combine activation and pip install using standard multi-line string (''' or "") separated by &&
                       sh '''
                           source venv/bin/activate
                           pip install --upgrade pip
                           pip install -r requirements.txt
                       '''
                   }
               }
            }
        }
        
        stage('Sonar Qube Analysis') {
            steps {
                withSonarQubeEnv('sonar-url') {
                    sh "${SONAR_HOME}/bin/sonar-scanner -Dproject.settings=./src/sonar-properties.project"
                }
            }
        }
        
        stage('Running Python Application') {
            steps {
                dir('src') {
                    // Call the python binary directly from the venv folder so it uses your installed dependencies
                    sh './venv/bin/python run.py'
                }
            }
        }
    }
    post {
        always {
            sh 'echo Learning is fun'
        }
    }
}





