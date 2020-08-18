pipeline {
   agent any
   environment 
    {
        VERSION = "${BUILD_NUMBER}"
        PROJECT = 'eks-demo'
        IMAGE = "$PROJECT:$VERSION"
        ECRURL = 'https://531359658382.dkr.ecr.ap-south-1.amazonaws.com/eks-demo'
        ECRCRED = 'ecr:ap-south-1:AWS_ECR'
    }   
    stages {
      stage('GetSCM') {
         steps {
            // Get some code from a GitHub repository
            git 'https://github.com/ChetanKumar07/Docker-ECR-EKS.git'
         }
         }
         stage('Image Build'){
             steps{
                 script{
                       docker.build('$IMAGE')
                 }
             }
         }
         stage('Push Image'){
         steps{
             script
                {
                   
                    docker.withRegistry(ECRURL, ECRCRED)
                    {
                        docker.image(IMAGE).push()
                    }
                }
            }
         }
        stage('To deploy on Kubernetes'){
        sshagent(['root-user1']){
            sh "ssh -o StrictHostKeyChecking=no root@13.126.194.4"
            sh "eksctl create cluster -f cluster.yaml --kubeconfig=C:\Users\{user}\.kube\config"
	    sh "kubectl apply -f deployment.yaml"
	    sh "kubectl apply -f service.yaml"
        }
      }
    }  
}
