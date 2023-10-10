pipeline{
  agent any
  tools{
    jdk 'java17'
    nodejs 'node16'
  }
  
  environment{
    SCANNER_HOME=tool 'sonar-scanner'
  }

  stages{
    stage('Clean workspace'){
      steps{
        cleanWs()
      }
    }

    stage('Checkout the code'){
      steps{
        git branch: 'main', url: 'https://github.com/srinivasarao-ceq/DevSecOps-Pipeline.git'
      }
    }

    stage('Sonarqube Analysis'){
      steps{
        withSonarQubeEnv(credentialsId: 'sonar-server'){
          sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Game \
                    -Dsonar.projectKey=Game '''
        }
      }
    }

    stage('Sonar-quality gate'){
      steps{
         waitForQualityGate abortPipeline: true, credentialsId: 'Sonar-token' 
      }
    }
  }
}
