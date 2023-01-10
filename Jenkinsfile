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
                script{
                    withSonarQubeEnv(credentialsId: 'SonarApiNew') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage('Quality Gate Status'){
            steps{

                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-ApiKey'
                }

            }
        }
    }
}