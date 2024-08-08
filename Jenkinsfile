node
{
    echo "Build number is : ${evn.BUILD_NUMBER}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
def mH= tool name :"maven 3.9.8"

stage('checkoutgitCode')
{

git branch: 'development', credentialsId: 'e0f2abd6-9977-4c4b-b29e-e19c1176e48c', url: 'https://github.com/ganeshraju1238/LT-maven-web-application.git'

}
timestamps {
    
stage('Build')
{
  sh  "${mH}/bin/mvn clean package"
}
stage('SonarQubeReport')
{
 sh  "${mH}/bin/mvn clean sonar:sonar"
}
stage('DeployArtifact')
{
 sh  "${mH}/bin/mvn clean sonar:sonar deploy"
}
stage('App Deploy to Tomcat')
{
sshagent(['49a57718-5760-48b3-a32a-2a7a28c45d94']) {

  sh "scp -o StrictHostKeyChecking=no target/Landmark.war ec2-user@13.201.118.112:/opt/apache-tomcat-9.0.91/webapps"
}
}
}
}
