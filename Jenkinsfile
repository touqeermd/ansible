  node {
  def mvn = '/opt/maven3/bin/mvn'
  def user = 'ec2-user@'
  def ip = '34.244.203.187'
  stage ('git checkout') { 
    git 'https://github.com/javahometech/my-app.git'
  }
  stage('Build'){
     sh "${mvn} clean package"
  }
  stage ('deploy to dev'){
   sh 'mv target/myweb*.war target/myweb.war'
   sshagent(['tomcat-dev']){
   sh "ssh -o StrictHostKeyChecking=no ${user}${ip} /opt/apache-tomcat-8/bin/shutdown.sh" 
   sh "ssh ${user}${ip} rm -rf /opt/apache-tomcat-8/webapps/myweb*"
   sh "scp target/myweb.war ${user}${ip}:/opt/apache-tomcat-8/webapps/"
   sh "ssh ${user}${ip} /opt/apache-tomcat-8/bin/startup.sh"
   mail bcc: '',
   body: '''deployed to dev
   Thanks
   Devops''',
   cc: '',
   from: '',
   replyTo: '', subject: 'deployed to dev', to: 'mohammed.tqr@gmail.com'
   slackSend color: 'good', message: 'welcome to jenkins'
   }    
  }
}
