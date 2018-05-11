pipeline {
    agent { label 'mvn-test' }
    tools { 
        maven 'Maven35' 
    }
    stages {
    stage ('Build') {
            steps {
                checkout scm
                echo 'This is a minimal pipeline.'
                sh 'mvn package'
            }
        }
    }
}
// test
