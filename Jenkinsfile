node{
  stage('SCM Checkout'){
    git 'https://github.com/Vinodh862/my-app.git'
  }
  stage('Compile-Package'){
    def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
  }
  stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
  stage('Build Docker Imager'){
    sh 'docker build -t mycard/myweb:0.0.5 .'
  }
  stage('Nexus Image Push'){
    sh "docker login -u admin -p Kiran@862 13.233.36.15:8083"
    sh "docker tag mycard/myweb:0.0.5 13.233.36.15:8083/damo:1.0.0"
    sh "docker push 13.233.36.15:8083/damo:1.0.0"
   }
//   stage('Docker Image Push'){
//     withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
//       sh 'docker login -u vinodh862 -p ${dockerPassword}'
//       }
//     sh 'docker push vinodh/myweb'
//   }
  stage('Remove Previous Container'){
    try{
        sh 'docker rm -f tomcattest'
    }catch(error){
       //  do nothing if there is an exception
   }
  }
  stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest mycard/myweb:0.0.5' 
   }
   
}
