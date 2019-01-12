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
                       sh 'sonar-scanner'
                }
             }

        stage('Test Reporting') {
              steps {
                jacoco(maximumBranchCoverage: '60')
              }
            }
        }
    }


         stage('SonarQube analysis') {
             steps {
               script {

                   scannerHome = tool 'SonarQube Scanner 3.2'
               }
               withSonarQubeEnv('SonarQube Scanner 3.2') {
                 sh "${scannerHome}/bin/sonar-scanner"
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