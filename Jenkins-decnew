pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git branch: 'main', credentialsId: 'jenkinshome', url: 'https://github.com/Manmohan1285/jenkins-CI-CD.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@172.31.92.162:/home/ec2-user/apache-tomcat-9.0.71/webapps/
                    
                    ssh ec2-user@172.31.92.162 /home/ec2-user/apache-tomcat-9.0.71/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.92.162 /home/ec2-user/apache-tomcat-9.0.71/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
