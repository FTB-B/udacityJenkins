pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'echo "Hello World"'
        sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
      }
    }

    stage('Staging') {
      parallel {
        stage('Testing') {
          steps {
            echo 'Entering Test stage'
          }
        }

        stage('Lint HTML') {
          steps {
            sh 'tidy -q -e *.html'
          }
        }

        stage('Integreation Test') {
          steps {
            echo 'Integration Test'
          }
        }

      }
    }

    stage('Deployment') {
      when {
        branch 'Deployment'
      }
      parallel {
        stage('Deployment') {
          steps {
            echo 'Deployment stage'
          }
        }

        stage('Upload to AWS') {
          steps {
            echo 'echo "Uploading content with AWS creds"'
            withAWS(credentials: 'FTC3', region: 'us-east-1') {
              s3Upload(bucket: 'static-jenkins-pipeline-ft', pathStyleAccessEnabled: true, payloadSigningEnabled: true, file: 'index.html')
            }

          }
        }

      }
    }

  }
}