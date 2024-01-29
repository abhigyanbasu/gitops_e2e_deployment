pipeline{
    agent any
    environment{
        APP_NAME = "complete-prodcution-e2e-pipeline" 
    }

   stages{
    stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
   }
   stage("Checkout from SCM"){
            steps{
                git branch: 'master', credentialsId: 'github', url: 'https://github.com/abhigyanbasu/gitops_e2e_deployment.git'
            }
   }
   stage("Update Deployment Tags"){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yaml
                   """
            }
   }

   stage("Push updated deployment to git"){
            steps{
                 sh """
                    git config --global user.name "vikram"
                    git config --global user.email "vikram@gmail.com"
                    git add deployment.yml
                    git commit -m 'Updated the deployment file' 
                   """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
                    sh "git push https://github.com/abhigyanbasu/gitops_e2e_deployment.git master "
                } 
              }
        }

   }
   
   
}