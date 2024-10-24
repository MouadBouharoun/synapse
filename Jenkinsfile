pipeline {    
    agent any
    environment {
        REPO_URL = 'https://github.com/MouadBouharoun/synapse.git'  
        SEMGREP_CONFIG = 'p/owasp-top-ten'  // Le profil Semgrep à utiliser
        LOCAL_DIR = 'synapse'  // Dossier local où le dépôt sera cloné
        SONAR_URL='http://127.0.0.1:9000'
        SONAR_TOKEN='sqp_cfe71569d438c5732e20c87f7e38c14d6d2354f'
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
                git branch: 'develop', url: "${REPO_URL}"
            }
      }  
      stage ('Bandit SAST Scan') {
          steps {
              
              sh "bandit -r . -l >> bandit.txt"
              
            }       
        }
      stage ('Semgrep SAST Scan') {
          steps {
              
              sh "semgrep --config ${SEMGREP_CONFIG} ."
              script {
                    def semgrepResult = sh(script: 'semgrep --config=p/python . | tee semgrep-result.log | grep "ERROR" || true', returnStatus: true)
                    if (semgrepResult != 0) {
                        error 'Semgrep found issues in the code.'
                    }
                }
            }       
        }  
      stage ('SonarQube SAST Scan') { 
        steps {
               sh "/opt/sonar-scanner/bin/sonar-scanner -Dsonar.projectKey=project-sonar -Dsonar.sources=. -Dsonar.host.url=${SONAR_URL} -Dsonar.login=${SONAR_TOKEN}"
           }  
        }   
      }
    
}  
