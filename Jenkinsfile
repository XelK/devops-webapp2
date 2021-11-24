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
      parallel {
        stage('Build') {
          steps {
            sh '''RELEASE=webapp.war
pwd
./gradlew build -PwarName=$RELEASE --info
ls -la build/libs/
cp ./build/libs/$RELEASE ./docker'''
          }
        }

        stage('P1') {
          steps {
            sh '''date 
                echo run parallel!'''
          }
        }

        stage('P2') {
          steps {
            sh 'echo Run parallel too!'
          }
        }

      }
    }

    stage('Packaging') {
      steps {
        sh '''pwd
cd ./docker
docker build -t xelk/webapp2:$BUILD_ID .
docker tag xelk/webapp2:$BUILD_ID xelk/webapp2:latest
docker images'''
      }
    }

    stage('Publish') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'ca-dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]){
            sh '''
docker login -u "$DOCKER_USERNAME" -p "$DOCKER_PASSWORD"
docker push xelk/webapp2:$BUILD_ID
docker push xelk/webapp2:latest
'''
          }
        }

      }
    }

  }
}