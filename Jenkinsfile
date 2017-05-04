node 
{


    def tomcatApp = "spring-music-tomcat"
    def nginxApp = "spring-music-nginx"
    def dhubUser = "harshals"
    def tomcatImageTag = "${dhubUser}/${tomcatApp}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    def nginxImageTag = "${dhubUser}/${nginxApp}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    checkout scm 
    stage 'Build Applications'
	sh("chmod 755 ./gradlew")
	sh("./gradlew wrapper")
	sh("./gradlew clean build")
	sh("./gradlew warNoStatic warCopy zipGetVersion zipStatic")
    stage 'Copy build artifacts'
	sh("cp build/distributions/*.war tomcat-docker-image/")
	sh("cp build/distributions/*.zip nginx-docker-image/*")
    stage 'Docker Image'
	sh("docker build -f tomcat-docker-image/Dockerfile -t ${tomcatImageTag} tomcat-docker-image")
	sh("docker build -f nginx-docker-image/Dockerfile -t ${nginxImageTag} nginx-docker-image")
    stage "Docker Image Push"
	sh("docker push ${tomcatImageTag}")
	sh("docker push ${nginxImageTag}")
}
