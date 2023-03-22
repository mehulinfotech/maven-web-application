node{
    try {
    notifyBuild('STARTED')
    def mavenHome = tool name: "maven 3.8.6"
    
    stage('Pull Code From Git'){
        git branch: 'development', credentialsId: '118884bc-0be5-4a45-a8fd-ff329c1d747a', url: 'https://github.com/mehulinfotech/maven-web-application.git'
    }
    stage('Build Package'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    /*stage('Change Permissions'){
	sh "chmod 777 target/maven-web-application.war"
    }*/
    stage('Generate SonarQube Report'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('Store to Nexus Repo'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('Deploy to Tomcat Server'){
        sshagent(['827e0e51-62d7-46ea-b421-0d2fc6a33410']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.10.236:/opt/tomcat8/webapps"
        }
    }
   }
	catch (e) {
		currentBuild.result = "FAILED"
		throw e}
	finally {
		notifyBuild(currentBuild.result)
   	}
}
def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
}
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
