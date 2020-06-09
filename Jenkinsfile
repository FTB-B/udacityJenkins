pipeline {
  agent any
  stages {
    stage('Build') {
      when {
        branch 'master'
      }
      steps {
        sh 'echo "Hello World"'
        sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
      }
    }

    stage('Lint HTML') {
      when {
        branch 'Staging'
      }
      steps {
        sh 'tidy -q -e *.html'
      }
    }

    stage('Upload to AWS') {
      when {
        branch 'Deployment'
      }
      steps {
        withAWS(region: 'us-east-1', credentials: 'FTC3') {
          sh 'echo "Uploading content with AWS creds"'
          s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file: 'index.html', bucket: 'static-jenkins-pipeline-ft')
        }

      }
    }

  }
}
