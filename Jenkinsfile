node 
{


    def tomcatApp = "spring-music-tomcat"
    def dhubUser = "harshals"
    def imageTag = "${dhubUser}/${tomcatApp}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    checkout scm 
    stage 'Build Applications'
	sh("chmod 755 ./gradlew")
	sh("./gradlew wrapper")
	sh("./gradlew clean build")
	sh("./gradlew warNoStatic warCopy zipGetVersion zipStatic")
	sh("cp build/distributions/*.war tomcat-docker-image/")
    stage 'Docker Image'
	sh("docker build -f tomcat-docker-image/Dockerfile -t ${imageTag} tomcat-docker-image")
    stage "Docker Image Push"
	sh("docker push ${imageTag})
}
