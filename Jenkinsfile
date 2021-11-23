pipeline {
  agent {
    node {
      label 'archlinux-docker'
    }
  }
  options {
    newContainerPerStage()
  }
  stages {
    stage('build') {
      parallel {
        node('archlinux-docker') {
          stage('lemonbar-xft-git') {
            steps {
              catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                sh '''
                cd lemonbar-xft-git && makepkg --nosign --syncdeps --noconfirm
                '''
              }
            }
          }
        }

        node('archlinux-docker') {
          stage('maim') {
            steps {
              catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                sh '''
                cd maim && makepkg --nosign --syncdeps --noconfirm
                '''
              }
            }
          }
        }
      }
    }

    stage('archive') {
      steps {
        archiveArtifacts(artifacts: '*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
      }
    }

  }
}
