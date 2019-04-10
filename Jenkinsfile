pipeline {
    agent any
    environment {
        registry = "naga488/samplejavamavenproject"
        registryCredential = "dockerhub"
                    // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus2"
            // This can be http or https
        NEXUS_PROTOCOL = "http"
            // Where your Nexus is running
        NEXUS_URL = "35.237.58.180:8081"
            // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "Releases"
            // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "nexus_credentials"
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
        
        stage("publish") {
            steps{
                //nexusPublisher nexusInstanceId: 'gcpnexus', nexusRepositoryId: 'myid', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/myMavenPipelineProject-0.0.1-SNAPSHOT.war']], mavenCoordinate: [artifactId: 'myMavenPipelineProject', groupId: 'com.org.manju', packaging: 'war', version: '2.23']]]
                //nexusPublisher nexusInstanceId: 'gcpnexus', nexusRepositoryId: 'myid3', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/myMavenPipelineProject-0.0.1-SNAPSHOT.war']], mavenCoordinate: [artifactId: 'hcl', groupId: 'org', packaging: 'war', version: '0.1']]]
               // nexusPublisher nexusInstanceId: 'gcpnexus', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/myMavenPipelineProject-0.0.1-SNAPSHOT.war']], mavenCoordinate: [artifactId: 'manju', groupId: 'com.org', packaging: 'war', version: '0.2']]]          
               // nexusPublisher nexusInstanceId: 'gcpnexus', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/myMavenPipelineProject-0.0.1-SNAPSHOT.war']], mavenCoordinate: [artifactId: 'hcl2', groupId: 'com.org', packaging: 'war', version: '0.3']]]
               //nexusPublisher nexusInstanceId: 'gcpnexus', nexusRepositoryId: 'testid', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/myMavenPipelineProject-0.0.1-SNAPSHOT.war']], mavenCoordinate: [artifactId: 'myproject', groupId: 'org.com.hcl', packaging: 'war', version: '0.1']]]
               //nexusPublisher nexusInstanceId: 'gcpnexus', nexusRepositoryId: 'testid', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/myMavenPipelineProject-0.0.1-SNAPSHOT.war']], mavenCoordinate: [artifactId: 'myproject1', groupId: 'org.com.hcl', packaging: 'war', version: '0.1']]]
                nexusPublisher nexusInstanceId: 'gcpnexus', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/myMavenPipelineProject-0.0.1-SNAPSHOT.war']], mavenCoordinate: [artifactId: 'hcl1.4', groupId: 'org.com', packaging: 'war', version: '0.1']]]

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

