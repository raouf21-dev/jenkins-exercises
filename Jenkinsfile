@Library("Jenkins-SharedLibrary")_

pipeline {
    agent any
    
    tools {
        nodejs 'node'
    }
   
    stages{
        stage('Check Committer') {
            steps {
                script {
                    checkCommiter()
                }
            }
        }
        stage("Increment version"){
            steps{
                script{
                    incrementVersion()
                }
            }
        }
        stage("Run tests"){
            steps{
                script{
                    testApp()
                }
            }
        }
        stage("Build & Push docker image"){
            steps{
                script{
                    buildPushDockerImage()
                }
            }
        }
        stage("Deploying image"){
            steps{
                script{
                    deployApp()
                }
            }
        }        
        stage("commit version update"){
            steps{
                script{
                    commitVersionUpdate()
                }
            }
        }
    }
}