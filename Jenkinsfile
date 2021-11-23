def packages = [
  "lemonbar-xft-git",
  "maim",
]

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
              sh '''
              cd lemonbar-xft-git && makepkg --nosign --syncdeps --noconfirm
              '''
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
              sh '''
              cd maim && makepkg --nosign --syncdeps --noconfirm
              '''
            }
            archiveArtifacts(artifacts: '**/*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
          }
        }
      }
    }

    stage('trigger repo update') {
      steps {
        build job: 'aur-packages/aur-update', parameters: [[$class: 'StringParameterValue', name: 'UPSTREAM_PROJECT', value: 'packages/main']]
      }
    }
  }
}
