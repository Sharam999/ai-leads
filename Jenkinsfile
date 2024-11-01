pipeline{
    agent any
    environment{
        TOMCAT_IP="172.31.47.56"
        TOMCAT_USER="ec2-user"
    }
    stages{
        stage("git checkout"){
            steps{
               git branch: 'main', credentialsId: 'github', url: 'https://github.com/Sharam999/ai-leads'
            }
        }
        stage("maven build"){
            steps{
               sh 'mvn clean package'
            }
        }
        stage("tomcat deploy"){
            steps{
                sshagent(['tomcat-creds']) {
                    //copy war file to tomcat
                sh "scp target/ai-leads.war ${TOMCAT_USER}@${TOMCAT_IP}:/opt/tomcat9/webapps"
                    //stop tomcat server
                sh  "ssh ${TOMCAT_USER}@${TOMCAT_IP} /opt/tomcat9/bin/shutdown.sh"
                    //start tomcat server
                sh "ssh ${TOMCAT_USER}@${TOMCAT_IP} /opt/tomcat9/bin/startup.sh"    
                }
            }
        }
    }
}
