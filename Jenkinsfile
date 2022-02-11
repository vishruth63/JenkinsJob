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
             sh "chmod +x myscript.sh"
             sh " sh myscript.sh ${BUILD_ID}" 
             sshagent(['ubuntu']) {
                 sh "scp -o StrictHostKeyChecking=no  newdep.yaml ubuntu@65.2.35.6:/home/ubuntu"
                 script{
                   try{
                      sh "ssh ubuntu@65.2.35.6 kubectl create -f newdep.yaml"
                   }
                   catch(error){
                        sh "ssh ubuntu@65.2.35.6 kubectl apply -f newdep.yaml"
                   }
                
                 }
             }           
            
          }
       }
 
    }  

}
