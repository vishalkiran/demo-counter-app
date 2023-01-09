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
    }
}