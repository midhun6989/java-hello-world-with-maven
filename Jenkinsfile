pipeline{
    agent any

    triggers {
        pollSCM('H/2 * * * 1-5')
    }

    stages{
        stage('build'){
            steps{
               sh "mvn package"
            }
        }

        stage('create release'){
            steps{
                withCredentials([string(credentialsId: 'GH-Token', variable: 'TOKEN')]) {
                   sh '''#!/bin/bash
                       curl -L \
                       -X POST \
                       -H "Accept: application/vnd.github+json" \
                       -H "Authorization: Bearer $TOKEN" \
                       -H "X-GitHub-Api-Version: 2022-11-28" \
                       https://api.github.com/repos/midhun6989/java-hello-world-with-maven/releases \
                       -d '{"tag_name":"v1.0.0","target_commitish":"master","name":"v1.0.0","body":"Description of the release","draft":false,"prerelease":false,"generate_release_notes":false}'
                      '''
                }        
            }
        }
        
    }
}
