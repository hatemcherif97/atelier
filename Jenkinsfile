pipeline{

    agent any
    tools {
        maven 'maven'
    }
    stages {

        stage('Git Checkout'){

            steps{

                script{

                    git branch: 'Develop', url: 'https://github.com/mohamed-chargui/cicd.git'
                }
            }
        }
        stage('UNIT testing'){

            steps{

                script{

                    sh 'mvn test'
                }
            }
        }
stage('Integration testing'){

            steps{

                script{

                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
       stage('Maven build'){

            steps{

                script{

                    sh 'mvn clean install'
                }
            }
        }
/*          stage('Static code analysis'){

            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'tokenCI') {

                        sh 'mvn clean package sonar:sonar'
                    }
        }
         }
    }*/
        
 stage('nexus_Repos') {
        steps{
            script{
                pom = readMavenPom(file: 'pom.xml')
                def pomversion = pom.version
                nexusRepo = pomversion.endsWith("SNAPSHOT") ? "democi-snapshot": "democicd-repo"
                nexusArtifactUploader artifacts:
                [
                 [
                    artifactId: 'springboot', 
                    classifier: '', 
                    file: 'target/Uber.jar', 
                    type: 'jar']], 
                    credentialsId: 'nexus_Auth', 
                    groupId: 'com.example', 
                    nexusUrl: 'localhost:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepo , 
                    version: "${pomversion}"
            }
         }
      }
   }
}
