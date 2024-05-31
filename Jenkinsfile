  
def remote = [:]
  remote.name = 'Docker Server'
  remote.host = '75.101.190.202'
  remote.user = 'ubuntu'
  remote.password = 'Mayclass202412#'
  remote.allowAnyHosts = true

pipeline {
  agent any

  environment {
       imagename = "badmancarteer/practice-class"
       registryCredential = 'DOCKERLOGIN'
       dockerImage = ''
       imagetag    = "${env.BUILD_ID}"
           }

     stages {

          stage('Building Docker image') {
               steps{
                   script {
                       dockerImage = docker.build imagename
                          }
               }
          }

          stage('Push Image To DockerHub') {
               steps{
                     script {
                         docker.withRegistry( '', registryCredential ) {
                         //dockerImage.push("$BUILD_NUMBER")
                         dockerImage.push("$imagetag")
                                              }
                         }
               }
          }

          stage('Deploy To Docker Server Using SSH') {
               steps{
                    script {
                         sshCommand remote: remote, command: "docker run --name may-docker-class -d -p 8080:80 badmancarteer/practice-class:3"
                    }
               }
          }

          stage('Remove Unused docker image') {
               steps{
                    //sh "docker rmi $imagename:$BUILD_NUMBER"
                    //sh "docker rmi $imagename:$imagetag"
                    sh "docker system prune -f"
                    }
          }
    
     }
}
