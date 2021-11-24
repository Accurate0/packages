def packages = [
  "lemonbar-xft-git",
  "maim",
  "rxvt-unicode-pixbuf-patched",
]

def jobs = [:]

for (int i = 0; i < packages.size(); i++) {
  jobs["${packages[i]}"] = {
    node('archlinux-docker') {
      stage(${packages[i]}) {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          dir(packages[i]) {
            sh "pwd && makepkg --nosign --syncdeps --noconfirm"
            archiveArtifacts(artifacts: '*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
          }
        }
      }
    }
  }
}

pipeline {
  agent none
  options {
    copyArtifactPermission('aur-packages/aur-update');
  }

  stages {
    stage('build packages') {
      steps {
        script {
          parallel jobs
        }
      }
    }

    // stage('repo update') {
    //   steps {
    //     build job: 'aur-packages/aur-update', parameters: [[$class: 'StringParameterValue', name: 'UPSTREAM_PROJECT', value: "${env.JOB_NAME}"]]
    //   }
    // }
  }
}
