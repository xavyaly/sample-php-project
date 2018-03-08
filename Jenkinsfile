pipeline {

  agent {
    node {
      label 'master'
    }
  }

  options {
    timestamps()
  }

  stages {
    stage('PHPUnit Test') {
      steps {
        echo 'Running PHPUnit...'
        sh '/bin/phpunit ${WORKSPACE}/src'
      }
    }
  }

  post {
    failure {
      slackSend "Build failed - Job: ${JOB_NAME} - Build No.: ${BUILD_NUMBER} - Build URL: (<${BUILD_URL}|Open>)"
    }
    changed {
      slackSend "Build status changed - Job: ${JOB_NAME} - Build No.: ${BUILD_NUMBER} - Build URL: (<${BUILD_URL}|Open>)"
    }
  }
}
