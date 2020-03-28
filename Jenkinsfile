// My First Jenkinsfile

node('master'){
 
   stage('Prepare'){
      checkout scm
    }
  
   stage('Build'){
      withMaven(
        maven : "M2_HOME"
    ){
      sh 'mvn clean install package'
    }
  }
  
   try{
   stage('Test'){
     withMaven(
       maven : "M2_HOME"
    ){
      sh 'mvn test'    
  }
  }  
 }
   finally{
        junit '**/target/surefire-reports/*.xml'
        jacoco()
    }
  
  stage('Docker_Build'){
   sh 'docker build -t martin1051/myapp:latest .'  
  }
 
 stage('Docker_Push'){
 withCredentials([string(credentialsId: 'DockerHubPass', variable: 'DockerHubCred')]) {
  sh "docker login -u martin1051 -p ${DockerHubCred}"
  }
  sh 'docker push martin1051/myapp:latest'
 }
}


