
Conversation with Gemini
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

                  sh "sudo apt-get update && apt-get install python3-venv -y" 

                  sh "python3 -m venv venv"

                  sh "source venv/bin/activate"

                  sh "pip install -r requirements.txt"

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

                dir('src'){

                sh 'python run.py'

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

That is a solid start to a Jenkins pipeline! However, there is a classic trap in the Installing Dependencies and Running Python Application stages that will cause your pipeline to fail or use the wrong Python environment.

The Hidden Trap: Shell Isolation
In Jenkins, every single sh command runs in its own completely separate shell process.

When you run sh "source venv/bin/activate", it activates the virtual environment inside that specific line and then immediately closes. When the next line (sh "pip install...") runs, it has already forgotten the virtual environment and will try to install packages globally (which usually fails due to permission errors). The same thing happens in your final stage—it won't use the virtual environment.

The Fix
To keep your virtual environment active, you need to combine the activation and the execution into the same sh block, or call the binaries inside the venv/bin/ directory directly.

Here is the optimized, clean version of your pipeline:

Groovy
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




Gemini is AI and can make mistakes.

