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
  stage('Ansible_Deployment'){
  sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i /opt/docker/hosts /opt/docker/simple.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'webapp/target/*.war, Dockerfile, simple.yml, hosts')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
  }
}

