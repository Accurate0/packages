def packages = [
  "lemonbar-xft-git",
  "maim",
]

pipeline {
  agent {
    node {
      label 'archlinux-docker'
    }
  }
  stages {
    script {
      projects.each { String package ->
        stage(package) {
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh '''
              cd ${package} && makepkg --nosign --syncdeps --noconfirm
              '''
            }
          }
        }
      }
    }
    // stage('lemonbar-xft-git') {
    //   steps {
    //     catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
    //       sh '''
    //       cd lemonbar-xft-git && makepkg --nosign --syncdeps --noconfirm
    //       '''
    //     }
    //   }
    // }

    // stage('maim') {
    //   steps {
    //     catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
    //       sh '''
    //       cd maim && makepkg --nosign --syncdeps --noconfirm
    //       '''
    //     }
    //   }
    // }

    stage('archive') {
      steps {
        archiveArtifacts(artifacts: '**/*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
      }
    }

    stage('trigger repo update') {
      script {
        projects.each { String package ->
          build job: 'aur-packages/aur-update', parameters: [[$class: 'StringParameterValue', name: 'UPSTREAM_PROJECT', value: package]]
        }
      }
    }
  }
}
