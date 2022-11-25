pipeline {
    agent any
    environment {
        PATH = "$PATH:/usr/share/maven/bin"
    }

    stages {

        stage('Git Checkout'){
            steps{
               git branch: 'main', url: 'https://github.com/smartobi/demo-counter-app.git' 
            }
        }

        stage('Unit Testing'){
            steps{
               sh 'mvn test' 
            }
        }

        stage('Integration testing'){
            steps{
               sh 'mvn verify -DskipUnitTests' 
            }
        }

        stage('Maven Build'){
            steps{
               sh 'mvn clean install' 
            }
        }

        stage('SonarQube analysis'){
            steps{
                scripts{
                     withSonarQubeEnv(credentialsId: 'sonarserver') {
                     sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar:sonar'
                 }
                }
               
            }
        }
    }
}