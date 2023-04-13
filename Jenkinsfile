pipeline{
    agent any
    tools{
        maven 'maven_3.9.0'
    }
    stages{
        stage('Pull Code from Github'){
            steps{
                echo "Get Code"
                git 'https://github.com/mehulinfotech/maven-web-application.git'
            }
        }
        stage('Build war package using maven'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Build docker image'){
            steps{
                sh 'docker build -t mehuldockerpractice/maven-web-application:${BUILD_NUMBER} .'
            }
        }
        stage('Docker login and Push image to docker hub'){
            steps{
                withCredentials([string(credentialsId: 'DockerHubPasswd', variable: 'DockerHubPasswd')]){
                    sh 'docker login -u mehuldockerpractice -p ${DockerHubPasswd}'
                    sh 'docker push mehuldockerpractice/maven-web-application:${BUILD_NUMBER}'
                }
            }
        }
        stage('Deploy to Dev Server'){
            steps{
                sshagent(['DevServerCredentials']){
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.44.252 docker rm -f mavenwebapp || true'
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.44.252 docker run -d --name mavenwebapp -p 8081:8080 mehuldockerpractice/maven-web-application:${BUILD_NUMBER}'
                }
            }
        }
    }
}
