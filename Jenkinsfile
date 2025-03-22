pipeline {
    agent any
    
    tools {
        nodejs 'node'
    }

    stages{
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
    }
}