pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-3.9.0/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/vishvanathpatil/Solutry.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
         }
        stage('SonarQube Analysis') {
            steps{
                withSonarQubeEnv('Sonar-Server-7.8') {
                    sh "mvn sonar:sonar"
                }
            }
        }
		stage('Code deploy') {
            steps{
				sshagent(['Tomcat_Server']) {
				  sh 'scp -o StrictHostKeyChecking=no target/Solutry.war ec2-user@44.195.78.239:/home/ec2-user/apache-tomcat-9.0.71/webapps'
				}
            }
        }
    }
}
