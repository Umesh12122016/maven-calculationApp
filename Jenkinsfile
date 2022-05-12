pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven3.8.1"
    }

    stages {
        stage('getscm') {
            steps {
                // Get some code from a GitHub repository
               git branch: 'dev', credentialsId: 'git-credentials', url: 'https://github.com/markondareddy/maven-calculationApp.git'

            }
        }
        stage('maven') {
             steps {
                // Run Maven on a Unix agent.
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
              withCredentials([sshUserPrivateKey(credentialsId: 'deployment-tomcat', keyFileVariable: '', passphraseVariable: '', usernameVariable: '')]) {
              sh "curl -v -u 'deployment-tomcat' -T /var/lib/jenkins/workspace/pipeline-project1/target/CalculationMavenApp.war 'http://ec2-54-227-222-119.compute-1.amazonaws.com:8080/manager/text/deploy?path=/pipeline_project1&update=true'"
              }
            }
        }
        
    }
    
     post {
        always {
          emailext attachLog: true, body: 'Jenkins URL - $JOB_URL ', recipientProviders: [developers()], subject: 'Jenkins - $JOB_NAME  - $BUILD_NUMBER ', to: 'bandi15713@gmail.com'  
            
        }
     }
    
    
}
