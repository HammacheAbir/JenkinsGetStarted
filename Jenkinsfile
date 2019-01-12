pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'gradle build'
        sh 'gradle jar'
        sh 'gradle javadoc '
      }
    }
    stage('Mail Notification') {
      steps {
        mail(subject: 'chaneges to master branch', body: 'Salam, ', from: 'fa_chenine@esi.dz', to: 'fa_hammache@esi.dz')
      }
    }
    stage('Code analysis') {
      parallel {
        stage('Code analysis') {
          steps {
           withSonarQubeEnv('SonarQube') {
            sh '''/Applications/sonarScanner/bin/sonar-scanner'''
            }
            waitForQualityGate()

          }
        }
        stage('Test Reporting') {
          steps {
            jacoco(maximumBranchCoverage: '60')
          }
        }
      }
    }
  }
}