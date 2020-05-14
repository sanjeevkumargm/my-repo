pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven/bin:$PATH"
    }
     stages{
          stage("Git Clone"){
               steps{
                   sh "rm -rf *"
                   git 'https://github.com/sanjeevkumargm/hello-world'
//                   git credentialsId: 'github', url: 'https://github.com/sanjeevkumargm/hello-world/'
               }
          }
          stage("Maven Build"){
               steps{
                   sh "mvn clean package"
               }
          }
         stage("Deploy-Tomcat"){
               steps{
                   sshagent(['tomcat']) {
                   sh """
                       scp -o StrictHostKeyChecking=no target/*.war centos@172.31.42.164:/opt/tomcat/webapps/
                       ssh centos@172.31.42.164 /opt/tomcat/bin/shutdown.sh
                       ssh centos@172.31.42.164 /opt/tomcat/bin/startup.sh
                       
                     """
                }

             }

         }
     }
}
