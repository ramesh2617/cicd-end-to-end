pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: '3717e5e0-d3d1-47a2-8dd3-72b98ec840d6', 
                url: 'https://github.com/ramesh2617/cicd-end-to-end.git',
                branch: 'main'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t mahiramesh2617/myargo:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker login -u mahiramesh2617 -p Mahi@2617
                    docker push mahiramesh2617/myargo:${BUILD_NUMBER}
                    
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: '3717e5e0-d3d1-47a2-8dd3-72b98ec840d6', 
                url: 'https://github.com/ramesh2617/cicd-end-to-end.git',
                branch: 'main'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
        environment {
            GIT_REPO_NAME = "cicd-end-to-end"
            GIT_USER_NAME = "ramesh2617"
            GITHUB_TOKEN = "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"
             }
        steps {
            withCredentials([string(credentialsId: '3717e5e0-d3d1-47a2-8dd3-72b98ec840d6', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        git config user.email "krameshchennai3456@gmail.com"
                        git config user.name "ramesh1"
                        cat deploy.yaml
                        sed -i  's/20/${BUILD_NUMBER}/g' deploy.yaml
                        cat deploy.yaml
                        git init
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push --set-upstream origin main
                        '''                        
                    }
                }
            }
        }
    }

