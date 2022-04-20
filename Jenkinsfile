node{
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
    def mavenhome=tool name: "Maven 3.8.3"
    stage('Git')
    {
     git branch: 'development', credentialsId: '2450ee4a-a05c-422f-99bc-8e7025bbe08b', 
     url: 'https://github.com/I-Love-You-Uday/maven-web-application.git'   
    }//Git Closing
    
    stage('build')
    {
        sh "${mavenhome}/bin/mvn clean package"
    }
    
    stage('sonarqube')
    {
        sh "${mavenhome}/bin/mvn sonar:sonar"
    }
    
    stage('nexus')
    {
        sh "${mavenhome}/bin/mvn deploy"
    }
    
    stage('tomcat')
    {
        sshagent(['9a0a269b-0dc6-42b2-9d3c-3dc479735704'])
        {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.110.167.234:/opt/apache-tomcat-9.0.60/webapps/"
        }
    }
    stage('calljob')
    {
        build job: "Facebook-Maventype"
    }
}//Node closing
