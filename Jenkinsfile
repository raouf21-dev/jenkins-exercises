pipeline {
    agent any
    
    tools {
        nodejs 'node'
    }

    stages{
        stage("test"){
            steps{
                script{
                    sh "npm -v"
                }
            }
        }
    }
}