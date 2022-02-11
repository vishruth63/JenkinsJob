pipeline {
  agent any
    stages{
       stage("Build Docker Image"){
           steps{
             sh "docker build -t 966145/newapp:${BUILD_ID} ."
          }
       }
       stage("Push Image to Repo"){
          steps{
            withCredentials([string(credentialsId: 'dhpass', variable: 'pass')]) {
             sh "docker login -u 966145 -p ${pass}"
             sh "docker push 966145/newapp:${BUILD_ID}"
            }  
           
          }
       }
       stage("K8s Deployment"){
          steps{
          
             sshagent(['ubuntu']) {
                 sh "scp -o StrictHostKeyChecking=no  dep.yaml ubuntu@65.2.35.6:/home/ubuntu"
                 sh "kubectl create -f dep.yaml"
             }           
            
          }
       }
 
    }  

}
