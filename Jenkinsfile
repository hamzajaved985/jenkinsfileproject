pipeline {
    //def mavenHome = tool name: "maven3.8.3"
    agent any
    //properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([cron('* * * * *')])])
    triggers{
       cron('* * * * *')
    }
    stages{
        stage('checkout'){
            steps{
                git credentialsId: '1b8ae7d9-2936-49ae-bd9a-8aad40c245af', url: 'https://github.com/hamzajaved985/maven-web-application.git'
            }
        }
        stage ('build'){
            tools{
                maven "maven3.8.3"
            }
            steps{
                sh "mvn clean package"
            }
        }
        stage ('deployintocontainer'){
            steps{
            sshagent(['43db78c6-7cb1-46d8-96bf-f47c2e40f8fb']) {
            sh "scp -o StrictHostKeyChecking=no  target/maven-web-application.war ec2-user@3.144.33.76:/opt/apache-tomcat-9.0.54/webapps/"  
            }
            }            
        }
        stage('emailnotification'){
            steps{
                emailext body: '''success ,
i love you ''', subject: 'build is done janu', to: 'hamzajaved985@gmail.com'
                
            }
        }
    }    
}
