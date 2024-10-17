pipeline{
    agent any

    triggers {
        pollSCM('H/2 * * * 1-5')
    }

    environment {
        OWNER = 'midhun6989'
        REPO = 'java-hello-world-with-maven'
    }
    
    stages{
        stage('build'){
            steps{
               sh 'mvn package'
            }
        }

        stage('create release'){
            steps{
                withCredentials([string(credentialsId: 'GH-Token', variable: 'TOKEN')]) {
                   sh '''#!/bin/bash
                       DATA='{"tag_name":"v'$BUILD_NUMBER'","target_commitish":"'$BRANCH_NAME'","name":"v'$BUILD_NUMBER'","body":"Description of the release","draft":false,"prerelease":false,"generate_release_notes":false}'

                       curl -L \
                       -o release.json \
                       -X POST \
                       -H "Accept: application/vnd.github+json" \
                       -H "Authorization: Bearer $TOKEN" \
                       -H "X-GitHub-Api-Version: 2022-11-28" \
                       https://api.github.com/repos/$OWNER/$REPO/releases \
                       -d "$DATA"
                      '''
                }        
            }
        }

        stage('upload release'){
            steps{
                withCredentials([string(credentialsId: 'GH-Token', variable: 'TOKEN')]) {
                   sh '''#!/bin/bash
                       pushd target
                       zip artifact.zip jb-hello-world-maven-0.2.0.jar
                       popd
                       
                       RELEASE_ID=$(jq '.id' release.json)
                       
                       curl -L \
                       -X POST \
                       -H "Accept: application/vnd.github+json" \
                       -H "Authorization: Bearer $TOKEN" \
                       -H "X-GitHub-Api-Version: 2022-11-28" \
                       -H "Content-Type: application/octet-stream" \
                       "https://uploads.github.com/repos/$OWNER/$REPO/releases/$RELEASE_ID/assets?name=artifact.zip" \
                       --data-binary "@target/artifact.zip"
                      '''
                }        
            }
        }
        
    }
}
