def packages = [
  "lemonbar-xft-git",
  "maim",
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
            def package = packages[i]
            jobs["${package}"] = {
              stage("${package}") {
                when { changeset "${package}/*"}
                agent {
                  label 'archlinux-docker'
                }
                steps {
                  catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                      sh "cd ${package} && makepkg --nosign --syncdeps --noconfirm"
                  }
                  archiveArtifacts(artifacts: '**/*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
                }
              }
            }
          }
          parallel jobs
        }
      }
    }

    stage('trigger repo update') {
      steps {
        build job: 'aur-packages/aur-update', parameters: [[$class: 'StringParameterValue', name: 'UPSTREAM_PROJECT', value: "${env.JOB_NAME}/main"]]
      }
    }
  }
}
