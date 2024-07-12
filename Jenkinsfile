pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "register-app"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github-creds', url: 'https://github.com/Velkaandhans/CICD_Jen_K8_ArgoCD.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "velkaandhans"
                   git config --global user.email "velkaandhans@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-creds', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Velkaandhans/CICD_Jen_K8_ArgoCD_CI_Manifest-files.git"
                }
            }
        }
      
    }
}