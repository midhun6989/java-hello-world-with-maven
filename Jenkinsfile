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
    }
}
