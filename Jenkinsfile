@Library ("Jenkins-SharedLibrary")_
def gv
pipeline {
    agent any
    
    tools {
        nodejs 'node'
    }
   
    stages{
        stage('Check Committer') {
            steps {
                script {
                    gv.checkCommiter()
                }
            }
        }
        stage("Increment version"){
            steps{
                script{
                    gv.incrementVersion()
                }
            }
        }
        stage("Run tests"){
            steps{
                script{
                    gv.testApp()
                }
            }
        }
        stage("Build & Push docker image"){
            steps{
                script{
                    gv.buildPushDocker()
                }
            }
        }
        stage("Deploying image"){
            steps{
                script{
                    gv.deployApp()
                }
            }
        }        
        stage("commit version update"){
            steps{
                script{
                    gv.commitVersionUpdate()
                }
            }
        }
    }
}