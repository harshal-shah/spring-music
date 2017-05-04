node 
{


    def tomcatApp = "spring-music-tomcat"
    def dhubUser = "harshals"
    def imageTag = "${dhubUser}/${tomcatApp}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    checkout scm 
}
