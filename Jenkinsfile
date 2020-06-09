pipeline {
  agent any
  stages {
    stage('Build') {
      when {

        branch 'Developement'

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


    stage('Testing') {
      when {
        branch 'Staging'
      }
      parallel {
        stage('Testing') {
          steps {
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
    stage('Security Scan') {
              steps { 
                 aquaMicroscanner imageName: 'alpine:latest', notCompleted: 'exit 1', onDisallowed: 'fail'
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
