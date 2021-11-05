ansiColor('xterm') {
  node("linuxd1") {
    def img
    stage("Clone TestRIG repository") {
      checkout scm
    }
    stage("Build TestRIG builder docker image") {
      copyArtifacts filter: 'bsc-install-focal.tar.xz', fingerprintArtifacts: true, projectName: 'bsc-build'
      img = docker.build("ctsrd/testrig-builder-mv380", "-f ci/testrig-builder.Dockerfile --no-cache .")
    }
    stage("Push TestRIG builder docker image to docker hub") {
      docker.withRegistry('https://registry.hub.docker.com',
                          'docker-hub-credentials') {
        img.push("${env.BUILD_NUMBER}")
        img.push("latest")
      }
    }
  }
}
