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
    stage('Create RPM') {
      steps {
        sh '/usr/local/bin/fpm -s dir -t rpm -a all -n php-project -v 1.0 --iteration 1 ${WORKSPACE}/src/=/var/www/php-project'
      }
    }
    stage('Artifactory RPM upload'){
      steps {
        script{
          def server = Artifactory.server 'Practical Jenkins Artifactory'
          def uploadSpec = readFile 'pkg_resources/upload.json'
          def buildInfo = server.upload spec: uploadSpec
          server.publishBuildInfo buildInfo
        }
      }
    }
  }

  post {
      always {
        cleanWs notFailBuild: true
      }
  }
}
