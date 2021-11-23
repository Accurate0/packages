pipeline {
  agent {
    node {
      label 'archlinux-docker'
    }
  }
  stages {
    stage('lemonbar-xft-git') {
      node {
        label 'archlinux-docker'
      }
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh '''
          cd lemonbar-xft-git && makepkg --nosign --syncdeps --noconfirm
          '''
        }
      }
    }

    stage('maim') {
      node {
        label 'archlinux-docker'
      }
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh '''
          cd maim && makepkg --nosign --syncdeps --noconfirm
          '''
        }
      }
    }

    stage('archive') {
      steps {
        archiveArtifacts(artifacts: '**/*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
      }
    }
  }
}
