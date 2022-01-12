pipeline {
  agent any
  stages {
    stage('message') {
      steps {
        echo 'hello word'
      }
    }

    stage('Build') {
      steps {
        sh 'dotnet build eShopOnWeb.sln'
      }
    }

    stage('Test') {
      parallel {
        stage('Unit') {
          steps {
            sh 'dotnet test tests/UnitTests'
          }
        }

        stage('Integration') {
          steps {
            sh 'dotnet test tests/IntegrationTests'
          }
        }

        stage('Fonctionnel') {
          steps {
            warnError(message: 'Fonction problem') {
              sh 'dotnet test tests/FonctionnelTests'
            }

          }
        }

      }
    }

    stage('Deployment') {
      steps {
        sh 'dotnet publish eShopOnWeb.sln -o /var/aspnet'
        dir(path: '/var/aspnet') {
          archiveArtifacts(onlyIfSuccessful: true, artifacts: '*')
        }

      }
    }

  }
}