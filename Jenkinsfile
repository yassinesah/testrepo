pipeline {
    agent any

    triggers {
        pollSCM('* * * * *') // This checks the repo every minute
        // OR use GitHub webhook if integrated
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yassinesah/testrepo'
            }
        }

}