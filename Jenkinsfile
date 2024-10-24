pipeline {    
    agent any
    environment {
        REPO_URL = 'https://github.com/MouadBouharoun/synapse.git'  
        SEMGREP_CONFIG = 'p/owasp-top-ten'  // Le profil Semgrep à utiliser
        LOCAL_DIR = 'synapse'  // Dossier local où le dépôt sera cloné
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
      stage ('Semgrep SAST Scan') {
          steps {
              
              sh "semgrep --config ${SEMGREP_CONFIG} ."
              //script {
              //      def semgrepResult = sh(script: 'semgrep --config=p/python . | tee semgrep-result.log | grep "ERROR" || true', returnStatus: true)
              //      if (semgrepResult != 0) {
              //          error 'Semgrep found issues in the code.'
              //      }
               // }
            }
              
          }
      
      }
    
}  
