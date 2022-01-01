def packages = [
  "lemonbar-xft-git",
  "maim",
  "rxvt-unicode-pixbuf-patched",
]

def jobs = [:]

for (int i = 0; i < packages.size(); i++) {
  def p = packages[i]
  jobs["${p}"] = {
    node {
      stage("${p}") {
        checkout scm
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          dir("${p}") {
            sh "sudo pacman -Syu --noconfirm"
            sh "makepkg --nosign --syncdeps --noconfirm"
            archiveArtifacts(artifacts: '*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
          }
        }
      }
    }
  }
}

pipeline {
  agent none
  stages {
    stage('build packages') {
      steps {
        script {
          parallel jobs
        }
      }
    }
  }
}
