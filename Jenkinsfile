 pipeline{

agent any

tools {
maven 'maven 3.9.9'
}//tools

options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '10', numToKeepStr: '10')
  timestamps()
}

triggers {
  githubPush()
}//

stages{

stage('checkout'){
steps{
deleteDir()
//git branch: "${params.BranchName}", url: 'https://github.com/Darshanputtaswamy7/maven-web-application.git'
git branch: 'master', url: 'https://github.com/Darshanputtaswamy7/javaee7-simple-sample.git'
}// steps
}//stage

stage('build'){
steps{
parallel(
startmessage: {SendSlackNotifications('STARTED')},
clean: {sh 'mvn clean test'},
package: {sh 'mvn clean package'},
Print: {echo "${params.Name}"}
)//parallel
}//steps
}//stage

/*
/stage('build master'){
steps{
build job: 'master'
}//steps
}//stage
*/

}//stages

post {
  success {
     SendSlackNotifications(currentBuild.result)  
    // One or more stepss need to be included within each condition's block.
  }//success
  failure {
       SendSlackNotifications(currentBuild.result)
    // One or more stepss need to be included within each condition's block.
  }//failure
}//post

}//pipeline

def SendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

// Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
