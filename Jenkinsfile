pipeline {
  agent none
  options {
    copyArtifactPermission('aur-packages/aur-update');
  }

  stages {
    stage('build packages') {
      parallel {
        stage('lemonbar-xft-git') {
          agent {
            label 'archlinux-docker'
          }
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh 'sudo pacman -Syu --noconfirm'
              dir("lemonbar-xft-git") {
                sh 'makepkg --nosign --syncdeps --noconfirm'
              }
            }
            archiveArtifacts(artifacts: '**/*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
          }
        }

        stage('maim') {
          agent {
            label 'archlinux-docker'
          }
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh 'sudo pacman -Syu --noconfirm'
              dir("maim") {
                sh 'makepkg --nosign --syncdeps --noconfirm'
              }
            }
            archiveArtifacts(artifacts: '**/*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
          }
        }

        stage('rxvt-unicode-pixbuf-patched') {
          agent {
            label 'archlinux-docker'
          }
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh 'sudo pacman -Syu --noconfirm'
              dir("rxvt-unicode-pixbuf-patched") {
                sh 'makepkg --nosign --syncdeps --noconfirm'
              }
            }
            archiveArtifacts(artifacts: '**/*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
          }
        }
      }
    }

    stage('repo update') {
      steps {
        build job: 'aur-packages/aur-update', parameters: [[$class: 'StringParameterValue', name: 'UPSTREAM_PROJECT', value: "${env.JOB_NAME}"]]
      }
    }
  }
}
