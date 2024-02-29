pipeline {
    agent { label 'newagent1' }
    environment {
        APP_NAME = 'register-app-pipeline'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/claudedevops/gitops-register-app'
            }
        }

        stage('Update k8s Manifest & Push to Manifest Repo') {
            steps {
                script {
                    
                    sh """
                        cat deployment.yaml
                        sed -i 's/\${APP_NAME}:\${IMAGE_TAG}//g' deployment.yaml
                        cat deployment.yaml
                        git config --global user.name "claudedevops"
                        git config --global user.email "visionary_p@yahoo.com"
                        git add deployment.yaml
                        git commit -m 'Updated the deployment Manifest'
                       """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                        sh 'git push https://github.com/claudedevops/gitops-register-app.git HEAD:main'
                    }
                }
            }
        }
    }
}
