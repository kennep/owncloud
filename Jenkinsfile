def image = 'owncloud'
def serviceName = image
def registry = 'https://registry.wangpedersen.com'
def registryLogin = 'registry-login'

node {
  checkout scm

  docker.withRegistry(registry, registryLogin) {
    stage 'Build'
    def builtImage = docker.build(image, '-f 9.0/apache/Dockerfile --pull 9.0/apache')

    if (env.BRANCH_NAME == 'master') {
        stage 'Push'
        builtImage.push()
    }
  }

  stage 'Deploy'
  build job: '/restart-service', parameters: [string(name: 'service', value: serviceName)]
}
