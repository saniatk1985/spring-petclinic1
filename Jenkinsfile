pipeline {
    agent none
    // tools { 
    //     maven 'Maven35'
    // }
    stages {
        agent { label 'master' }
        stage ('Build') {
                steps {
                    checkout scm
                    echo 'This is a minimal pipeline'
                    //sh 'mvn package'
                }
        }
        agent { label 'master' }
        stage ('Deploy EC2') {
                steps {
                    ansiColor('xterm') {
                        ansiblePlaybook(
                            playbook: 'ec2/test.yml',
                            //inventory: 'path/to/inventory.ini',
                            //credentialsId: 'sample-ssh-key',
                            colorized: true)
                    }
                }
        }
    }
}
