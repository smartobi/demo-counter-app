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
                   script{
                     withSonarQubeEnv(credentialsId: 'sonar-token') {
                     sh 'mvn clean package sonar:sonar'
                 }
                }
               
            }
        }
        
        stage('Quality Gate status'){
            steps{
              
                  waitForQualityGate abortPipeline: false, credentialsId: 'sonaqube-token'
               
                }
               
            }
        
        stage('upload war file to nexus'){
            steps{
                script{
                    nexusArtifactUploader artifacts:
                     [
                        [
                            artifactId: 'springboot',
                            classifier: '', 
                            file: 'target/Uber.jar',
                            type: 'jar'
                            ]
                            
                    ],
                    credentialsId: 'nexus-auth', 
                    groupId: 'com.example', 
                    nexusUrl: '18.119.135.232:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'demoapp-release', 
                    version: '1.0.0'
                }
            }
        }

        

        
    }
}