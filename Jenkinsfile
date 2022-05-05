node{
    
echo "Job Name is: {env.JOB_NAME}"
echo "Build Number is: {env.BUILD_NUMBER}"
echo "Node name is: {env.NODE_NAME}"
echo "Jenkins home Dir is: {env.JENKINS_HOME}"

def mavenHome = tool name: 'Maven-3.8.5'

//Get the sorce code from Github Repo
stage('checkOutCode'){
git branch: 'development', credentialsId: 'ef22bdf4-b8bb-41ee-a362-247ccc54b2ce', url: 'https://github.com/eshwardevops/maven-web-application.git'
}

//Do the Build usiing the maven tool
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

//Generate SonarQube report
stage('SonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

//Upload to Atrifact repository
stage('UploadtoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}

//Deploy to Tomcat
stage('DeployToTomcat'){
sshagent(['0e549818-dc12-479b-9c72-cd9d05871fbb']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.126.36.53:/opt/apache-tomcat-9.0.62/webapps"  
}

}

}
