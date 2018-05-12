pipeline {
    agent none
    // tools { 
    //     maven 'Maven35'
    // }
    stages {
        // stage ('Build') {
        //     agent { label 'master' }
        //         steps {
        //             checkout scm
        //             echo 'This is a minimal pipeline'
        //             //sh 'mvn package'
        //         }
        // }
        // stage ('Deploy EC2') {
        //     agent { label 'master' }
        //         steps {
        //             ansiColor('xterm') {
        //                 ansiblePlaybook(
        //                     playbook: 'ec2/test.yml',
        //                     //inventory: 'path/to/inventory.ini',
        //                     //credentialsId: 'sample-ssh-key',
        //                     colorized: true)
        //             }
        //         }
        // }
        stage ('Ansible AH') {
            agent { label 'mvn-test' }
                steps {
                    ansiColor('xterm') {
                        ansiblePlaybook(
                            playbook: '/tmp/test.yml',
                            //inventory: 'path/to/inventory.ini',
                            //credentialsId: 'sample-ssh-key',
                            colorized: true)
                    }
                }
        }
    }
}
