pipeline {
    agent any
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
        
    }
     environment {
           SERVER_ADDRESS = "ec2-user@3.82.139.1"
    //     def pom = readMavenPom file: "pom.xml"
    //     NAME = "${pom.artifactId}"  
    //     VERSION = "${pom.properties.revision}"
    //     ARTIFACT = "${NAME_ACTUAL}-${VERSION}"

     }


    stages {
        stage('checkOutCode') {
            steps{
                 git branch: 'dev', credentialsId: 'ab4323c5-ab85-4eb3-9bf0-219843e590de',
                 url: 'https://github.com/Sbsharma0897/nexus-demo/' 
                 }
            }
        stage('build') {
            steps{
                 sh "mvn clean package"
            }
        }  
        stage('deployToNexus') {
            steps{
                 sh "mvn clean deploy"
            }
        }  
        stage('deployToMyServer') {
            steps {
                sshagent(['dev']) {
                    sh "echo sandeep"
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@3.82.139.1 ls -la /var/lib/jenkins/workspace/"
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@3.82.139.1  mkdir -p /home/ec2-user/nexus-home "
                    sh "scp -r -o StrictHostKeyChecking=no target/*.jar ec2-user@3.82.139.1:/home/ec2-user/nexus-home"  
                }
            }
        }
    }
    
    post {
        success {
            emailext (
                            subject: "PCF-JENKINS - FAILED !!! - Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                            body: "Last commit resulted in FAILED Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                           
                            attachLog: true,
                            to: "sandeep@platformcommons.com"
                    )
            }
        failure {
            emailext (
                            subject: "PCF-JENKINS - FAILED !!! - Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                            body: "Last commit resulted in FAILED Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                           
                            attachLog: true,
                            to: "sandeep@platformcommons.com"
                    )
            }
        }

}