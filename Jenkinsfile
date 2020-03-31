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
  
  stage('Ansible_Deployment'){
  sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: '', sourceFiles: './**')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
  }
}

