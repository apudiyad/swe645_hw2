pipeline {
   environment {
        registry = "apudiyad/student-survey-form"
        registryCredential = 'dockerhub'
      dockerImageTag = 'latest'
    }
   agent any

   stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DockerHub_Credentials') {
                        docker.image('apudiyad/homework2:latest').push()
                    }
                }
            }
        }
      stage('Push Image to Dockerhub') {
         steps {
            echo 'pushing to image to docker hub'
            script{
               docker.withRegistry('',registryCredential){
                  sh "docker push apudiyad/student-survey-form:${env.BUILD_NUMBER}"
               }
            }
         }
      }

      stage('Deploying to Rancher to single node(deployed in 3 replicas)') {
         steps {
            echo 'deploying on kubernetes cluster'
            script{
               //sh "docker pull srinathsilla/student-survey-form:${env.BUILD_NUMBER}"
               sh "kubectl --kubeconfig /home/ubuntu/.kube/config set image deployment/hw2-cluster container-0=apudiyad/student-survey-form:${BUILD_NUMBER}"
            }
         }
      }
   }
}
