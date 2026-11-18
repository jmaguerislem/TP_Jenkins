pipeline {
  agent any
  options { timestamps() }

  environment {
    PATH = "C:\\Users\\jmagu\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;${env.PATH}"
    // DOCKER_HOST = 'npipe:////./pipe/docker_engine'
    IMAGE = 'islemjmaguer/monapp'
    TAG   = "build-${env.BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Docker Build') {
      steps {
        bat 'docker version'
        bat "docker build -t %IMAGE%:%TAG% ."
      }
    }

    stage('Smoke Test') {
      steps {
        bat """
          docker rm -f monapp_test 2>nul || ver > nul
          docker run -d --name monapp_test -p 8081:80 %IMAGE%:%TAG%
          ping -n 4 127.0.0.1 > nul
          curl -I http://localhost:8081 | find "200 OK"
          if errorlevel 1 ( docker rm -f monapp_test & exit /b 1 )
          docker rm -f monapp_test
        """
      }
    }

    stage('Push (Docker Hub)') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                          usernameVariable: 'USER',
                                          passwordVariable: 'PASS')]) {
          bat """
            echo %PASS%| docker login -u %USER% --password-stdin
            docker tag %IMAGE%:%TAG% %IMAGE%:latest
            docker push %IMAGE%:%TAG%
            docker push %IMAGE%:latest
          """
        }
      }
    }

    stage('Deploy') {
      steps {
        bat """
          docker compose -p tp_jenkins pull
          docker compose -p tp_jenkins up -d
          docker ps
        """
      }
    }
  }

  post {
    success { echo 'Build+Test+Push+Deploy OK' }
    failure { echo 'Build/Tests/Push/Deploy KO' }
  }
}