pipeline {    
    agent any
    tools {
       maven 'Maven'
    } 
    stages {
      stage ('Initialise') {
         steps {
            sh '''
          echo "PATH = ${PATH}"
          echo "M2_HOME = ${M2_HOME}"
          ''' 
         }
        
      }
      stage('Checkout') {
            steps {
                git branch: 'develop', url: 'https://github.com/MouadBouharoun/synapse.git' 
            }
      }
      
      stage('Install Semgrep') {
            steps {
                // Create a virtual environment
                sh 'python3 -m venv venv'
                
                // Activate the virtual environment and install semgrep
                sh '''
                . venv/bin/activate
                pip install semgrep
                '''
            }
        }
      stage('Run Semgrep') {
            steps {
                // Activate virtual environment and run Semgrep
                sh '''
                . venv/bin/activate
                semgrep --config=p/python --json > semgrep-report.json
                '''
            }
        }
      
      stage ('Semgrep SAST Scan') {
          steps {
              //sh 'pip install semgrep'
              //sh 'semgrep --config=p/python --json > semgrep-report.json'
              script {
                    def semgrepResult = sh(script: 'semgrep --config=p/python . | tee semgrep-result.log | grep "ERROR" || true', returnStatus: true)
                    if (semgrepResult != 0) {
                        error 'Semgrep found issues in the code.'
                    }
                }
            }
              
          }
      
      }
    
}  
