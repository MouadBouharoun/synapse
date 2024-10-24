pipeline {    
    agent any
    environment {
        REPO_URL = 'https://github.com/MouadBouharoun/synapse.git'
        SYNAPSE_DIR = '/var/lib/jenkins/synapse/'
        SEMGREP_CONFIG = 'p/owasp-top-ten'  
        LOCAL_DIR = 'synapse'  
        SONAR_URL='http://127.0.0.1:9000'
        SONAR_TOKEN='sqa_692ffdbb7f191a24157882dbcb9c8cb0febe9f32'
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
             sh '''
                   bandit -r ${SYNAPSE_DIR} -f json -o bandit_report_2.json || true
                   cat bandit_report_2.json
              '''
        
            }       
        }
      stage ('Semgrep SAST Scan') {
          steps {
              
              sh "semgrep --config ${SEMGREP_CONFIG} ${SYNAPSE_DIR}"
              //script {
              //      def semgrepResult = sh(script: 'semgrep --config=p/python . | tee semgrep-result.log | grep "ERROR" || true', returnStatus: true)
              //      if (semgrepResult != 0) {
              //          error 'Semgrep found issues in the code.'
              //      }
              }
            }       
          }  
      stage ('SonarQube SAST Scan') { 
        steps {
               sh "/opt/sonar-scanner/bin/sonar-scanner -Dsonar.projectKey=project-2-sonar -Dsonar.sources=${SYNAPSE_DIR} -Dsonar.host.url=${SONAR_URL} -Dsonar.login=${SONAR_TOKEN}"
           }  
        }  
      }
    
}  
