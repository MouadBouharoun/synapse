pipeline {    
    agent any
    tools {
       maven 'Maven'
    } 
    stages {
      stage ('Initialise') {
         step {
           sh '''
              echo "PATH = ${PATH}"
              echo "M2_HOME = ${M2_HOME}"
           '''
         }
        
      }
    }
}  
