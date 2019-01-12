pipeline {
  agent any
  stages {
    stage('build') {

      steps {
        sh 'gradle build'
        sh 'gradle javadoc '
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/*'
      }

      post {
              failure {
                mail(subject: 'Repported changes', body: 'Salam, some changes occured and the build failed', from: 'fa_chenine@esi.dz', to: 'chenineazeddine5@gmail.com')

              }

              success {
                mail(subject: 'Repported changes', body: 'Salam, some changes occured and the build successeded', from: 'fa_chenine@esi.dz', to: 'chenineazeddine5@gmail.com')

              }

         }
    }

    stage('Code analysis') {
      parallel {
        stage('Code analysis') {
          steps {
                   withSonarQubeEnv('My SonarQube Server') {
                       sh 'sonar-scanner'
                      } // SonarQube taskId is automatically attached to the pipeline context
                }
             }

        stage('Test Reporting') {
              steps {
                jacoco(maximumBranchCoverage: '60')
              }
            }
        }
    }


    stage('Deployment') {
      steps {
        sh 'gradle uploadArchives'
      }
    }


    stage('slack notification') {
      steps {
        slackSend(message: 'Salam, Project deployed')
      }
    }


  }
}