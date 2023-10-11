pipeline {
   environment {
        registry = "apudiyad/homework2"
        registryCredential = 'dockerhub'
        dockerImageTag = 'latest'
    }
   agent any

   stage('Build') {
    steps {
        echo 'Building...'
        script {
            sh 'rm -rf *.war'
            sh 'jar -cvf student-survey-form.war -C src/main/webapp/ .'
            // Define the Docker image name including the registry
            def dockerImage = "apudiyad/homework2:${env.BUILD_NUMBER}"
            // Build the Docker image
            docker.build(dockerImage)
        }
    }
}


      stage('Push Image to Dockerhub') {
         steps {
            echo 'pushing to image to docker hub'
            script{
               docker.withRegistry('',registryCredential){
                  sh "docker push apudiyad/homework2:${env.BUILD_NUMBER}"
               }
            }
         }
      }

      stage('Deploying to Rancher to single node(deployed in 3 replicas)') {
         steps {
            echo 'deploying on kubernetes cluster'
            script{
               //sh "docker pull srinathsilla/student-survey-form:${env.BUILD_NUMBER}"
               sh "kubectl --kubeconfig /home/ubuntu/.kube/config set image deployment/hw2-cluster container-0=apudiyad/homework2:${BUILD_NUMBER}"
            }
         }
      }
   }
}
