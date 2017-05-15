podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'docker', image: 'docker:latest', ttyEnabled: true, command: 'cat')],
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {

  node('mypod') {
      stage('Build and Push'){
          checkout scm
          container('docker'){
            stage ('Build Docker image'){
              sh 'which docker; docker version'
              def imageName = "${env.DOCKERHUB_USER}/${env.CONTAINER_IMAGE}:${env.BUILD_TAG}"
              git url: 'git://github.com/pblaas/app1.git app'
              sh "docker build -t ${imageName}  ."
              def img= docker.image(imageName)

              stage ('Push Docker image to registry'){
                docker.withRegistry("https://registry.hub.docker.com", "docker-registry") {
                  img.push()
                }
              }
            }
          }
      }
    }
}
