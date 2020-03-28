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
  
  stage('Sonar'){
  withMaven(
    maven : "M2_HOME"
    ){
    sh 'mvn clean install sonar:sonar'   
  }
  }
  
  stage('Docker_Build'){
  sh 'docker build -t martin1051/myapp:latest .'  
  }
}
}

