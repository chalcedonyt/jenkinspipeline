pipeline {
    agent any

    tools {
        maven 'DefaultMaven'
    }

    parameters {
        string(
            name: 'tomcat_staging',
            defaultValue: 'tomcat:8080',
            description: 'The staging server IP/hostname',
        )
        string(
            name: 'tomcat_production',
            defaultValue: 'tomcat-production:8080',
            description: 'The production server IP/hostname'
        )
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean package'
                sh "docker build -t tomcatapp:${env.BUILD_ID} ."
            }
            post {
                success {
                    echo 'Archiving'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        // stage ('Deployments') {
        //     parallel {
        //         stage('Deploy to staging') {
        //             steps {
        //                 sshagent('e8a97da1-3841-4c2c-9bf0-969d17172fd6') {
        //                     sh 'scp **/target/*.war ${tomcat_staging}:/var/lib/tomcat7/webapps/'
        //                 }
        //             }
        //         }

        //         stage('Deploy to production') {
        //             steps {
        //                 sshagent('e8a97da1-3841-4c2c-9bf0-969d17172fd6') {
        //                     sh 'scp **/target/*.war ${tomcat_production}:/var/lib/tomcat7/webapps/'
        //                 }
        //             }
        //         }
        //     }
        // }
    }
}