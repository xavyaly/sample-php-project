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
    stage('JIRA') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        script {
          response = jiraAddComment site: 'practical-jenkins-jira', idOrKey: env.GIT_BRANCH, comment: "Build result: Job - ${JOB_NAME} Build number - ${BUILD_NUMBER} Build URL - ${BUILD_URL}"
        }
      }
    }
  }
}
