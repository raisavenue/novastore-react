pipeline {
  agent any
  
  options { 
    timestamps() 
  }
  
  environment {
    BUILD_DIR  = 'build'
    DEPLOY_DIR = '/var/www/html'
  }
  
  stages {
    stage('Checkout') {
      steps { 
        checkout scm 
      }
    }
    
    stage('Install Dependencies') {
      steps {
        sh '''
          npm install
        '''
      }
    }
    
    stage('Build React App') {
      steps {
        sh '''
          npm run build
        '''
        archiveArtifacts artifacts: "${BUILD_DIR}/**", fingerprint: true
      }
    }
    
    stage('Deploy to Nginx') {
      steps {
        sh '''
          mkdir -p "${DEPLOY_DIR}"
          rm -rf "${DEPLOY_DIR:?}/"*
          cp -r "${BUILD_DIR}/." "${DEPLOY_DIR}/"
        '''
      }
    }
  }
  
  post {
    success {
      echo 'React app deployed successfully! Open http://localhost:8081'
    }
    failure {
      echo 'Build or deployment failed. Check the logs above.'
    }
  }
}