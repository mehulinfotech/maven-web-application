node{
    def mavenHome = tool name: "maven 3.8.6"
    
    stage('Pull Code From Git'){
        git branch: 'development', credentialsId: '118884bc-0be5-4a45-a8fd-ff329c1d747a', url: 'https://github.com/mehulinfotech/maven-web-application.git'
    }
    stage('Build Package'){
        sh "chmod 777 target/maven-web-application.war"
    }
    stage('Give full permissions'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('Generate SonarQube Report'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('Store to Nexus Repo'){
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('Deploy to Tomcat Server'){
        sshagent(['827e0e51-62d7-46ea-b421-0d2fc6a33410']) {
        sh "scp -p -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.10.236:/opt/tomcat8/webapps"
    }
        
    }
}
