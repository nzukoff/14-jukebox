pipeline {
  agent any

  environment {
	  DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
  }

  stages {
    stage('greeting') {
      steps {
        sh '''
          echo "hello moon"
'''
      }
    }
    stage('run test') {
      steps {
        sh '''
          docker-compose build
          docker-compose run web ./scripts/setup.sh rails test
'''
      }
    }
    stage('docker clean, first removes containers, second removes images') {
      steps {
        sh '''
          docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)
          docker system prune -f
'''
      }
    }
     stage('build image') {
      steps {
        sh '''
          docker build -t nzukoff/jukebox:$BUILD_NUMBER .
'''
      }
    }
    stage('docker login and push') {
      steps {
        sh '''
          docker login -u nzukoff -p $DOCKER_PASSWORD
          docker push nzukoff/jukebox:$BUILD_NUMBER
'''
      }
    }       
  }
}
