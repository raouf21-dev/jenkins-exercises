pipeline {
    agent any
    
    tools {
        nodejs 'node'
    }
   
    stages{
        stage('Check Committer') {
            steps {
                script {
                    def committer = sh(script: "git log -1 --pretty=format:'%ae'", returnStdout: true).trim()
                    def ignoredUsers = "jenkins@example.com"
                    
                    if (ignoredUsers.contains(committer)) {
                        echo "Commit by ignored user: ${committer}. Skipping build."
                        currentBuild.result = 'ABORTED'
                        return
                    }
                }
            }
        }
        stage("Increment version"){
            steps{
                script{
                    // enter app directory.
                    dir("app") {
                        // update application version in the package.json file with one of these release types: patch, minor or major
                        // This command updates the minor version in package.json and ensures no Git commands are executed in the background, preventing automatic commits or tags in your Jenkins Pipeline
                        sh "npm version minor -—no-git-tag-version"

                        // read the updated version from the package.json file
                        def packageJson = readJSON file: 'package.json'
                        def version = packageJson.version

                        // set the new version as part of IMAGE_NAME
                        env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                    }

                }
            }
        }
        stage("Run tests"){
            steps{
                script{
                    dir('app'){
                        sh "npm install"
                        sh "npm run test"
                    }
                }
            }
        }
        stage("Build & Push docker image"){
            steps{
                script{
                    echo "Building the image..."
                    withCredentials([usernamePassword(credentialsId: "docker-hub-creds", usernameVariable: 'USER', passwordVariable: 'PWD')]) {
                        sh "docker build -t santana20095/node-app:${IMAGE_NAME} ."
                        sh "echo $PWD | docker login -u $USER --password-stdin"
                        sh "docker push santana20095/node-app:${IMAGE_NAME}"
                    }
                }
            }
        }
        stage("Deploying image"){
            steps{
                script{
                    echo "Deploying image..."
                }
            }
        }        
        stage("commit version update"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: "git-creds", usernameVariable: 'USER', passwordVariable: 'PWD') ]){
                        sh "git config --global user.email jenkins@example.com"
                        sh "git config --global user.name jenkins"

                        sh "git status"
                        sh "git branch"
                        sh "git config --list"

                        sh "git remote set-url origin https://${USER}:${PWD}@github.com/raouf21-dev/jenkins-exercises.git"
                        sh 'git add .'
                        sh "git commit -m 'ci version bump'"
                        sh "git push origin HEAD:master"
                    }
                }
            }
        }
    }
}