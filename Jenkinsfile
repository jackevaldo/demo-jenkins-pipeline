pipeline {
  agent { docker { image 'node:lts' } }

  options {
    timestamps()
    ansiColor('xterm')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install dependencies') {
      steps {
        sh '''
          if [ -f package-lock.json ]; then
            npm ci
          else
            npm install
          fi
        '''
      }
    }

    stage('Run tests') {
      steps {
        sh 'npm test'
      }
      post {
        always {
          junit testResults: 'reports/**/*.xml', allowEmptyResults: true
        }
      }
    }
  }

  post {
    success {
      echo '✅ Tests exécutés avec succès !'
    }
    failure {
      echo '❌ Échec du pipeline, vérifie les logs Jenkins.'
    }
  }
}
