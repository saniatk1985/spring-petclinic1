pipeline {
    agent { label 'mvn-test' }
    tools { 
        maven 'Maven35',
        ansible 'ansible25'
    }
    stages {
    stage ('Build') {
            steps {
                checkout scm
                echo 'This is a minimal pipeline'
                //sh 'mvn package'
            }
        }
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
