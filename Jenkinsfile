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
                  sh "apt-get install pythom3.12-venv -y" 
                  sh "python3 -m venv venv"
                  sh "source venv/bin/activate"
                  sh "pip install requirements.txt"
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
                dir('src'){
                sh 'python run.py'
            }
            }
        }
    }
}
