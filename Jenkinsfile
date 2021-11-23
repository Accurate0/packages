pipeline {
  agent {
    node {
      label 'archlinux-docker'
    }
  }
  stages {
    stage('lemonbar-xft-git') {
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh '''
          cd lemonbar-xft-git && makepkg --nosign --syncdeps --noconfirm
          '''
        }
      }
    }

    stage('maim') {
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
