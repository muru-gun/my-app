node{
   stage('SCM Checkout'){
	 git 'https://github.com/muru-gun/my-app.git'
   }
   
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }

   stage('Build Docker Imager'){
   sh 'docker build -t murugan88/myweb:0.0.2 .'
   }

   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u murugan88 -p ${dockerPassword}"
    }
   sh 'docker push murugan88/myweb:0.0.2'
   }

   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest saidamo/myweb:0.0.2' 
   }
}
