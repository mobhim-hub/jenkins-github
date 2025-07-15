pipeline {
  agent { label 'docker-builder' }

  environment {
    DEPLOY_KEY = credentials('remote-server-key')
  }

  triggers {
    githubPush()
  }

  stages {
    stage('Clean Workspace') {
      steps {
        cleanWs()
      }
    }

    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/mobhim-hub/jenkins-github.git',
            credentialsId: 'github-creds'
      }
    }

    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build'
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }

    
  }

  post {
    always {
      echo 'Build finished.'
    }
    success {
      echo '✅ SUCCESS'
    }
    failure {
      echo '❌ FAILED'
    }
  }
}
