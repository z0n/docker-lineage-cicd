pipeline {
  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('Run build') {
      steps {
        configFileProvider([configFile(fileId: 'lineageos-env', targetLocation: './.env')]) {}
        sh 'mkdir -p ./zips'
        sh 'mkdir -p ./logs'
        sh 'docker-compose -f docker-compose.yml up --force-recreate --renew-anon-volumes --abort-on-container-exit --exit-code-from lineageos-build'
      }
    }

    stage('Archive build artifacts') {
      steps {
        archiveArtifacts 'zips/**, logs/**'
      }
    }

    stage('Clean up') {
      steps {
        sh 'docker-compose -f docker-compose.yml down --rmi local'
      }
    }

  }
}
