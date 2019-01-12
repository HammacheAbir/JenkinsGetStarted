pipeline {
  agent any
  stages {
    stage('build') {
      post {
        failure {
          mail(subject: 'Repported changes', body: 'Salam, some changes occured and the build failed', from: 'fa_chenine@esi.dz', to: 'chenineazeddine5@gmail.com')

        }

        success {
          mail(subject: 'Repported changes', body: 'Salam, some changes occured and the build successeded', from: 'fa_chenine@esi.dz', to: 'chenineazeddine5@gmail.com')

        }

      }
      steps {
        sh 'gradle build'
        sh 'gradle javadoc '
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/*'
      }
    }
    stage('Code analysis') {
      parallel {
        stage('Code analysis') {
          steps {
            withSonarQubeEnv('SonareQube') {
              sh 'sonar-scanner'
            }
            waitForQualityGate true
            def qualitygate = waitForQualityGate()
            if (qualitygate.status != "OK") {
                     error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
                   }

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
        slackSend(message: 'hello')
      }
    }
  }
}