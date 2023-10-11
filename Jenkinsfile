node{
   stage('SCM Checkout'){
     git 'https://github.com/aselvaraj123/Devops-project'
   }
   stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('Build Docker Image'){
   sh 'docker build -t anthoni1995/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u anthoni1995 -p ${dockerPassword}"
    }
   sh 'docker push anthoni1995/myweb:0.0.2'
   }
  stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 18.130.238.43:8083"
   sh "docker tag anthoni1995/myweb:0.0.2 18.130.238.43:8083/anthoni1995:1.0.0"
   sh 'docker push 18.130.238.43:8083/anthoni1995:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest anthoni1995/myweb:0.0.2' 
   }
}
}
