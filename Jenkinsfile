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
            sh 'tidy -q -e *.html'
            echo 'Entering Test stage'
          }
        }

        stage('Lint Html') {
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
      parallel {
        stage('Deployment') {
          steps {
            withAWS(region: 'us-east-1', credentials: 'FTC3') {
              sh 'echo "Uploading content with AWS creds"'
            }

          }
        }

        stage('Upload to AWS') {
          steps {
            echo 'echo "Uploading content with AWS creds"'
            s3Upload(bucket: 'static-jenkins-pipeline-ft', pathStyleAccessEnabled: true, payloadSigningEnabled: true)
          }
        }

      }
    }

  }
}