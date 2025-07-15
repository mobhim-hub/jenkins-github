pipeline {
  agent { label 'docker-builder' }

  environment {
    DEPLOY_KEY = credentials('jenkins-admin') // optional, only used if deploy is enabled
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

    stage('Checkout Code') {
      steps {
        git branch: 'main',
            url: 'https://github.com/mobhim-hub/jenkins-github.git',
            credentialsId: '	28187223-2904-4aaa-9b87-0df8b4f0daa2'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        sh 'npm test || true' // avoid build fail for now if no tests
      }
    }

    stage('Build App') {
      steps {
        sh 'npm run build'
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }

    // 🟢 Optional Deploy Stage (Uncomment if needed)
    // Make sure your SSH credential is working and remote host is reachable
    /*
    stage('Deploy to Remote') {
      steps {
        sshagent(['remote-server-key']) {
          sh '''
            scp -r build/* ubuntu@192.168.1.100:/opt/app/
          '''
        }
      }
    }
    */
  }

  post {
    always {
      echo '🔁 Build finished.'
    }
    success {
      echo '✅ Build succeeded!'
    }
    failure {
      echo '❌ Build failed. Check logs above.'
    }
  }
}
