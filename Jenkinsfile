pipeline {
  agent {
    node {
      label 'jenkins-node02'
    }

  }
  stages {
    stage('Clone') {
      steps {
        git(url: 'https://github.com/XelK/devops-webapp2', branch: 'master')
      }
    }

    stage('Build') {
      steps {
        sh '''whoami



date
echo $PATH
pwd
ls -la
./gradlew build
--info'''
      }
    }

    stage('Publish') {
      steps {
        archiveArtifacts(artifacts: 'build/libs/*.war', fingerprint: true, onlyIfSuccessful: true)
      }
    }

  }
}