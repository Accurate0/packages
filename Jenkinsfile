pipeline {
  agent {
    docker {
      image 'jenkins/agent:latest-archlinux-jdk11'
    }

  }
  stages {
    stage('build') {
      parallel {
        stage('lemonbar-xft-git') {
          steps {
            sh '''
            sudo pacman -Syu base-devel --noconfirm
            cd lemonbar-xft-git && makepkg --nosign --syncdeps --noconfirm
            '''
          }
        }

        stage('maim') {
          steps {
            sh '''
            sudo pacman -Syu base-devel --noconfirm
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
