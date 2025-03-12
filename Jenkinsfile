pipeline {
    agent {
        label 'My-Jenkins-Agent'
    }
    
    environment {
        APP_NAME = "devops-003-pipeline-aws"
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                //   checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mimaraslan/gitops-devops-003-pipeline-aws']])
                git branch: 'master', credentialsId: 'github', url: 'https://github.com/mimaraslan/gitops-devops-003-pipeline-aws'
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
                   git config --global user.name "mimaraslan"
                   git config --global user.email "mimaraslan@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/mimaraslan/gitops-devops-003-pipeline-aws master"
                }
            }
        }

      
    }
}