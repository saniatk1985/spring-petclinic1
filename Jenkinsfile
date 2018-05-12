pipeline {
    agent none
    // tools { 
    //     maven 'Maven35'
    // }
    stages {
        // stage ('Build') {
        //     agent { label 'mvn-test' }
        //         steps {
        //             checkout scm
        //             sh 'mvn package'
        //             sh 'aws s3 cp target/*.jar s3://tlk-demo2/pc.jar'
        //         }
        // }

        // stage ('Setup BD') {
        //     agent { label 'bd-slave' }
        //         steps {
        //             sh 'curl -O https://raw.githubusercontent.com/anatolek/spring-petclinic/master/ec2/db-playbook.yml'
        //             sh 'curl -O https://raw.githubusercontent.com/anatolek/spring-petclinic/master/ec2/mysql.cnf.j2'
        //             ansiColor('xterm') {
        //                 ansiblePlaybook(
        //                     playbook: 'db-playbook.yml',
        //                     colorized: true)
        //             }
        //         }
        // }
        
        stage ('Setup PC') {
            agent { label 'app-slave' }
                steps {
                    sh 'curl -O https://raw.githubusercontent.com/anatolek/spring-petclinic/master/ec2/app-playbook.yml'
                    ansiColor('xterm') {
                        ansiblePlaybook(
                            playbook: 'app-playbook.yml',
                            colorized: true)
                    }
                }
        }
    }
}
