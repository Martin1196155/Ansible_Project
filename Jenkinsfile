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
   sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i hosts simple.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: '**/target', sourceFiles: '**/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
}


