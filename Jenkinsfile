pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven 3.8.1"
    }

    stages {
        stage('getscm') {
            steps {
                // Get some code from a GitHub repository
               git branch: 'dev', credentialsId: 'git-credentials', url: 'https://github.com/Umesh12122016/maven-calculationApp.git'

            }
        }
        stage('maven') {
             steps {
                // Run Maven on a linux agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                
            }
        }
        
        stage('Artifact') {
            steps {
                // maven artifacts
               archiveArtifacts 'target/*.war'
              // archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }
        
        stage('Deploy') {
            steps {
              withCredentials([sshUserPrivateKey(credentialsId: 'linux_tomcat_credentials', keyFileVariable: '', passphraseVariable: '', usernameVariable: '')]) {
              sh "curl -v -u 'linux_tomcat_credentials' -T /var/lib/jenkins/workspace/pipeline-project1/target/CalculationMavenApp.war 'http://ec2-35-88-255-15.us-west-2.compute.amazonaws.com:8080/manager/text/deploy?path=/pipeline_project1&update=true'"
              }
            }
        }
        
    }
    
     post {
        always {
          emailext attachLog: true, body: 'Jenkins URL - $JOB_URL ', recipientProviders: [developers()], subject: 'Jenkins - $JOB_NAME  - $BUILD_NUMBER ', to: 'hari.k2030@gmail.com'  
            
        }
     }
    
    
}
