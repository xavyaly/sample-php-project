pipeline {
    
    agent {
        node {
            label 'master'
            }
        }   // end of agent
        
    options {
        timestamps()
        }   // end of options
        
    stages {
        stage('PHPUnit Test') {
            steps {
                echo 'Running PHPUnit...'
                sh '/bin/phpunit ${WORKSPACE}'
                }
            }   // end of PHPUnit Test
            
        stage(‘SonarQube analysis’) {
            steps {
                withSonarQubeEnv(‘SonarQubeImplementation’) {
                    sh ‘echo “sonar.projectkey=production:phptest” > ${WORKSPACE}/sonar-project.properties’
                    sh ‘echo “sonar.sources=.” >> ${WORKSPACE}/sonar-project.properties’
                    sh ‘/opt/sonarqube-scanner/bin/sonar-scanner’
                }
            }
        }   // end of SonarQube analysis
    }   // end of stages
}   // end of pipeline


