pipeline {
  agent any
  options { timestamps() }
  stages {

    stage('Cloner le dépôt') {
      steps {
        git url: 'https://github.com/jmaguerislem/TP_Jenkins.git', branch: 'main', credentialsId: 'github-token'
      }
    }

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Étape 1 : Vérification du dépôt') {
      steps {
        bat 'echo === Étape 1 : Vérification du dépôt ==='
        bat 'git status'
      }
    }

    stage('Étape 2 : Afficher le contenu du projet') {
      steps {
        bat 'echo === Étape 2 : Afficher le contenu du projet ==='
        bat 'dir'
      }
    }

    stage('Build') {
      steps {
        bat 'echo === Build du projet ==='
        bat 'dir'
      }
    }

    stage('Test') {
      steps {
        bat 'echo === Verification des fichiers ==='
        bat 'if not exist index.html exit 1'
        bat 'if not exist style.css exit 1'
        bat 'echo Tests OK'
      }
    }

    stage('Étape 3 : Simuler un déploiement local') {
      steps {
        bat 'echo === Étape 3 : Simuler un déploiement local ==='
        bat 'echo Le fichier index.html est prêt à être affiché'
      }
    }

    stage('Deploy') {
      steps {
        bat 'echo === Deploy ==='
        bat 'if not exist C:\\DevOps\\deploy mkdir C:\\DevOps\\deploy'
        bat 'copy /Y index.html C:\\DevOps\\deploy\\'
        bat 'copy /Y style.css C:\\DevOps\\deploy\\'
        bat 'echo Deploiement termine dans C:\\DevOps\\deploy'
      }
    }

    stage('Étape 4 : Fin du build') {
      steps {
        bat 'echo === Étape 4 : Fin du build ==='
        bat 'echo SUCCESS'
      }
    }
  }
  post {
    success { echo 'Build OK' }
    failure { echo 'Build KO' }
  }
}