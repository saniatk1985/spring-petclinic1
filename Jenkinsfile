pipeline {
    agent none
    tools { 
        maven 'Maven35'
    }
    stages {
        stage ('Build') {
            agent { label 'master' }
                steps {
                    checkout scm
                    sh 'mvn package'
                    sh 'aws s3 cp target/*.jar s3://tlk-demo2/pc.jar'
                }
        }

        // stage ('Ansible AH') {
        //     agent { label 'mvn-test' }
        //         steps {
        //             ansiColor('xterm') {
        //                 ansiblePlaybook(
        //                     playbook: '/home/ubuntu/test.yml',
        //                     //inventory: 'path/to/inventory.ini',
        //                     //credentialsId: 'sample-ssh-key',
        //                     colorized: true)
        //             }
        //         }
        // }
    }
}
