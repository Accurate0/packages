def packages = [
  "lemonbar-xft-git",
  "maim",
  "rxvt-unicode-pixbuf-patched",
]

def jobs = [:]

for (int i = 0; i < packages.size(); i++) {
  def p = packages[i]
  jobs["${p}"] = {
    node('archlinux-docker') {
      stage("${p}") {
        checkout scm
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          dir("${p}") {
            sh "ls && makepkg --nosign --syncdeps --noconfirm"
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
