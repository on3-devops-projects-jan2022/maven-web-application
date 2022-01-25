node
{
    def mavenHome = tool name: "maven-3.8.4"
    
    echo "GitHub Branch: ${env.BRANCH_NAME}"
    echo "Jenkins Node: ${env.NODE_NAME}"
    echo "Jenkins Home: ${env.JENKINS_HOME}"
    echo "Job Name: ${env.JOB_NAME}"
    echo "Job Build: ${env.BUILD_NUMBER}"
  
    
    stage('Code Checkout')
    {
        git branch: 'development', url: 'https://github.com/on3-devops-projects-jan2022/maven-web-application.git'
    }
    
    stage('build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('Upload Artifact to Nexus')
    {
        sh "${mavenHome}/bin/mvn clean deploy"
    }
    
    stage('Deploy to Tomcat Application Server')
    {
        sshagent(['07654ef8-5ffa-43c1-9336-edee2b4fbfe9']) {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.161.194.27:/opt/tomcat/webapps/"

        }
    }
    
    stage ('sendEmailNotification'){
        emailext body: 'kind regards', subject: 'Job with Build Number -${env.BUILD_NUMBER} completed', to: 'on3.mind21@gmail.com'
    }
}
