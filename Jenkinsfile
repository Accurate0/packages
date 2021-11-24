def packages = [
  "lemonbar-xft-git",
  "maim",
  "rxvt-unicode-pixbuf-patched",
]

def jobs = [:]

pipeline {
  agent none
  options {
    copyArtifactPermission('aur-packages/aur-update');
  }

  stages {
    stage('build packages') {
      steps {
        script {
          for (int i = 0; i < packages.size(); i++) {
            echo i
            jobs["${packages[i]}"] = {
              stage("${packages[i]}") {
                node('archlinux-docker') {
                  catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    echo packages[i]
                    echo i
                    dir(packages[i]) {
                      sh "pwd && makepkg --nosign --syncdeps --noconfirm"
                      archiveArtifacts(artifacts: '*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
                    }
                  }
                }
              }
            }
          }
          parallel jobs
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
