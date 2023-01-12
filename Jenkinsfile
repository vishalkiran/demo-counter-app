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
        stage('Upload jar file to nexus'){
            steps{

                script{

                    def readPomVersion = readMavenPom file: 'pom.xml'

                    def nexusRepo = readMavenPom.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-release"

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
                    nexusUrl: '13.127.181.40:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'nexusRepo', 
                    version: "${readPomVersion.version}"
                }
            }
        }
    }
}