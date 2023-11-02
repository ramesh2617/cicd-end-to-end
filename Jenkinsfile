pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: 'f87a34a8-0e09-45e7-b9cf-6dc68feac670', 
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
                    docker push mahiramesh2617/myargo:${BUILD_NUMBER}
                    docker login -u mahiramesh2617 -p Mahi@2617
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: '50456577-9339-4b4d-9dc5-0767f2936197', 
                url: 'https://github.com/ramesh2617/cicd-end-to-end.git',
                branch: 'main'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
        environment {
            GIT_REPO_NAME = "cicd-end-to-end"
            GIT_USER_NAME = "ramesh2617"
             }
        steps {
            withCredentials([string(credentialsId: '50456577-9339-4b4d-9dc5-0767f2936197', variable: 'GITHUB_TOKEN')]) {
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

