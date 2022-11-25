pipeline {
    agent any

    stages {

        stage('Git Checkout'){
            steps{
               git branch: 'main', url: 'https://github.com/smartobi/demo-counter-app.git' 
            }
        }
    }
}