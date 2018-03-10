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
      steps {
        script {
          def issue = jiraGetIssue idOrKey: env.GIT_BRANCH, site: 'practical-jenkins-jira'
          if (issue.code.toString() == '200') {
            response = jiraAddComment site: 'practical-jenkins-jira', idOrKey: env.GIT_BRANCH, comment: "Build result: Job - ${JOB_NAME} Build number - ${BUILD_NUMBER} Build URL - ${BUILD_URL}"
          } else {
            def issueInfo = [fields: [ project: [key: 'PJD'],
                             summary: "Review build ${GIT_BRANCH}",
                             description: 'Review changes for build ${GIT_BRANCH}',
                             issuetype: [name: 'Task']]]
            response = jiraNewIssue issue: issueInfo, site: 'practical-jenkins-jira'
          }
        }
      }
    }
  }
}
