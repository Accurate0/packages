pipeline {
  agent { dockerfile true }

  stages {
    stage('build') {
      parallel {
        stage('lemonbar-xft-git') {
          steps {
            sh '''
            cd lemonbar-xft-git && makepkg --nosign --syncdeps --noconfirm
            '''
          }
        }

        stage('maim') {
          steps {
            sh '''
            cd maim && makepkg --nosign --syncdeps --noconfirm
            '''
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
