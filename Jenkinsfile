pipeline {
    agent any
    environment {
        registry = "naga488/samplejavamavenproject"
        registryCredential = "dockerhub"
    }
    stages {
        stage(‘SCM’) { 
            steps {
                git 'https://github.com/ManjulaNaga/simpleJavaMavenProject.git'
            }
        }
        stage(‘Maven’) { 
            steps {
                 sh 'mvn clean compile package '
            }
        }
        /*stage('Sonarqube') {
            environment {
                scannerHome = tool 'sonarqube'
            }
            steps {
                    sleep(10)
                    withSonarQubeEnv('sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                    timeout(time:3, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }   
            }
        }
        */
          stage('Building image') {
              steps{
                script {
                  dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
              }
            }
            stage('Deploy Image') {
              steps{
                script {
                  docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                  }
                }
              }
            }
        
        /*stage('create container') {
            steps {
                sh 'docker run naga488/samplejavamavenproject:51'+
            }
        }*/
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
        }
    }
        
      }
}

