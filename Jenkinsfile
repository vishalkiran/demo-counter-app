pipeline {
    agent any

    tools {
        maven 'maven3.8.6'
    }


    stages{

        stage('Git Checkout'){
            steps{
               git branch: 'main', url: 'https://github.com/vishalkiran/demo-counter-app.git' 
            }
        }
        stage('UNIT Testing'){
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
        stage('Static code analysis'){
            steps{
                withSonarQubeEnv(credentialsId: 'SonarQube-ApiKey') {
                    sh 'mvn clean package sonar:sonar'
                 
                }
        }   }
    }
}