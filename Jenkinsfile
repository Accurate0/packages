pipeline {
  agent any
  stages {
    stage('lemonbar-xft-git') {
      parallel {
        stage('lemonbar-xft-git') {
          steps {
            sh '''sudo pacman -Syu --noconfirm
BUILDDIR=/tmp makepkg -p lemonbar-xft-git/PKGBUILD'''
          }
        }

        stage('maim') {
          steps {
            sh '''sudo pacman -Syu --noconfirm
BUILDDIR=/tmp makepkg -p maim/PKGBUILD'''
          }
        }

      }
    }

    stage('artifacts') {
      steps {
        archiveArtifacts(artifacts: '*.pkg.tar.zst', fingerprint: true, onlyIfSuccessful: true)
      }
    }

  }
}