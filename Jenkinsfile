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
                        echo "This is the version $version"
                        echo "$IMAGE_NAME"
                    }

                }
            }
        }
    }
}