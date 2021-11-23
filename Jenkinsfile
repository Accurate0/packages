pipeline {
  agent {
    docker {
      image 'jenkins/agent:latest-archlinux-jdk11'
    }

  }
  stages {
    stage('build') {
      parallel {
        stage('lemonbar-xft-git') {
          steps {
            dockerNode(image: 'localhost:5000/archbuild') {
              sh 'echo test'
            }

          }
        }

        stage('maim') {
          steps {
            dockerNode(image: 'localhost:5000/archbuild') {
              sh 'echo test2'
            }

          }
        }

      }
    }

    stage('archive') {
      steps {
        archiveArtifacts(artifacts: '*.pkg.tar.zst', onlyIfSuccessful: true, fingerprint: true)
      }
    }

  }
}