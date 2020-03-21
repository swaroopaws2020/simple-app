pipeline {
    agent any
    tools {
        maven 'maven3.6.3'
    }
    stages {
         stage('Build'){
            steps{
                script{
                    def pom = readMavenPom file:'pom.xml'
                    def version = "App" + "-" + pom.version
                    currentBuild.displayName = "App" + "-" + version 
                }
                sh 'mvn clean install'
                sh 'ls'
                //echo 'Building the stage'
            }
        }
        stage('Stages Running in Parallel / Scans') {
            failFast true
            parallel {
                stage('Sonar Scan') {
                    steps {
                        echo "Stage1 executing"
                        sleep 10
                    }
                }
                stage('Fortify Scan') {
                    steps {
                        echo "Stage2 executing"
                        sleep 10
                    }
                }
                /*stage('Stage3') {
                    steps {
                        echo "Stage3 executing"
                        sleep 10
                    }                    
                }*/
            }
        }
        stage('Build and push Docker image'){
            steps{
                echo 'Building and pushing docker image to artifactory'
            }
        }
        stage('Deploy to Dev environment '){
            steps{
                echo 'Deploying to dev environment'
            }
        }
        stage('Test'){
            steps{
                timeout(time: 10, unit: 'SECONDS') {
                    input message:"Promoto to test" , ok:"OK"
                    
                }
                
            echo 'Promote to test'
            }
        }
        
    }
}
