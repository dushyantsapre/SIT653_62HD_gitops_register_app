pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "sit653_62hd"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/dushyantsapre/SIT753_62HD_gitops_register_app'
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
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh """
                       # Configure Git user for this repository only.
                       git config user.name 'dushyantsapre'
                       git config user.email 'dushyantsapre@yahoo.com'
                       
                       # Add and commit changes
                       git add deployment.yaml
                       git commit -m "Updated Deployment Manifest"
                       
                       # Use the credentials variables for pushing
                       git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/dushyantsapre/SIT753_62HD_gitops_register_app main
                    """
                }
            }
        }
    }
}
