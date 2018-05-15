pipeline {

    agent none

    // tools { 
    //     maven 'Maven35'
    // }

    stages {

    //     stage ('Build') {
    //         agent { label 'mvn-test' }
    //             steps {
    //                 checkout scm
    //                 sh 'mvn package'
    //                 sh 'aws s3 cp target/*.jar s3://tlk-demo2/pc.jar'
    //                 sh """aws ec2 terminate-instances --instance-ids \
    //                     \$(curl -s http://169.254.169.254/latest/meta-data/instance-id)"""
    //             }
    //     }
        stage('Integration Test') {
            failFast true
            parallel {

                stage ('Setup BD instance') {
                    agent { label 'db-slave' }
                        steps {
                            sh 'curl -O https://raw.githubusercontent.com/anatolek/spring-petclinic/master/ec2/db-playbook.yml'
                            sh 'curl -O https://raw.githubusercontent.com/anatolek/spring-petclinic/master/ec2/mysql.cnf.j2'
                            ansiColor('xterm') {
                                ansiblePlaybook(
                                    playbook: 'db-playbook.yml',
                                    colorized: true)
                            }
                        }
                }

                stage ('Setup APP instance') {
                    agent { label 'app-slave' }
                        steps {
                            sh 'curl -O https://raw.githubusercontent.com/anatolek/spring-petclinic/master/ec2/app-playbook.yml'
                            ansiColor('xterm') {
                                ansiblePlaybook(
                                    playbook: 'app-playbook.yml',
                                    colorized: true)
                            }
                            sh 'aws s3 cp pc.jar s3://tlk-demo2/pc-${BUILD_NUMBER}.jar'
                        }
                }

            }
        }

        stage ('Terminate instances') {
            agent { label 'master' }
                steps {
                    sh """for i in \$(aws ec2 describe-instances \
                    --filters 'Name=tag:Name,Values=jenkins-slave*' \
                    --query 'Reservations[*].Instances[*].InstanceId' \
                    --output=text); do aws ec2 terminate-instances \
                    --instance-ids \$i; done"""
                }
        }
    }
}
