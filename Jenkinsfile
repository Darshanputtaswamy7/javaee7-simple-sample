pipeline {

agent any

tools {
  maven 'maven 3.9.10'
}//tools
//hi1
options {
timestamps()
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
}
triggers {
  githubPush()
}



stages {
  
  stage('checkout') {
    steps {
	       slackSend color: '#FFFF00', message: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - STARTED: <${env.BUILD_URL}|(open)>"
      git branch: 'master', url: 'https://github.com/Darshanputtaswamy7/javaee7-simple-sample.git'
    }//steps
  }//stage
  
  stage('build') {
  steps {
    parallel(
	Rununittestcases:{
	sh "mvn clean test"
	},
	build:{
	sh "mvn clean package"
	}//parallel2
	)//parallel
  }//steps
}//stage

}//stages

post {
  success {
     deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: '3f23cced-c9c8-4ac5-84b6-1e2efc9fed60', path: '', url: 'http://13.204.69.235:8080')], contextPath: null, onFailure: false, war: '**/*.war'
 slackSend color: '#00FF00', message: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - ${currentBuild.currentResult}: <${env.BUILD_URL}|(open)> "
      }//success

  unsuccessful {
     slackSend color: '#FF0000', message: "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} - FAILED: <${env.BUILD_URL}|(open)>"
  }//unsuccessful
  
  }//post




}//pipeline
