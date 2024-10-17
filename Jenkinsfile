pipeline{
    agent any

    triggers {
        pollSCM('H/2 * * * 1-5')
    }

    stages{
        stage('build'){
            steps{
               bat 'mvn package'
            }
        }
    }
}
