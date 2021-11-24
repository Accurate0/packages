pipeline {
  agent none
  options {
    copyArtifactPermission('aur-packages/aur-update');
  }

  stages {
    stage('build packages') {
      parallel {
        stage('lemonbar-xft-git') {
          when {
            anyOf {
              changeset "lemonbar-xft-git/**/*"
              changeset "Jenkinsfile"
            }
          }
          agent {
            label 'archlinux-docker'
          }
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh '''
              sudo pacman -Syu --noconfirm
              cd lemonbar-xft-git && makepkg --nosign --syncdeps --noconfirm
              '''
            }
            archiveArtifacts(artifacts: '**/*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
          }
        }

        stage('maim') {
          when {
            anyOf {
              changeset "maim/**/*"
              changeset "Jenkinsfile"
            }
          }
          agent {
            label 'archlinux-docker'
          }
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh '''
              sudo pacman -Syu --noconfirm
              cd maim && makepkg --nosign --syncdeps --noconfirm
              '''
            }
            archiveArtifacts(artifacts: '**/*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
          }
        }

        stage('rxvt-unicode-pixbuf-patched') {
          when {
            anyOf {
              changeset "rxvt-unicode-pixbuf-patched/**/*"
              changeset "Jenkinsfile"
            }
          }
          agent {
            label 'archlinux-docker'
          }
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh '''
              sudo pacman -Syu --noconfirm
              cd rxvt-unicode-pixbuf-patched && makepkg --nosign --syncdeps --noconfirm
              '''
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
