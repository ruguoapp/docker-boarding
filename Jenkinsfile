node {
  PROJECT_NAME = 'boarding'
  checkout scm
  GIT_COMMIT_HASH = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
  GIT_COMMIT_MESSAGE = sh(script: "git log -1 --pretty=%s", returnStdout: true).trim()

  stage('Build') {
    docker.withRegistry('http://docker.in.ruguoapp.com', 'e206c2b0-728e-408b-8549-475c47419210') {
      image = docker.build("${PROJECT_NAME}:${env.BRANCH_NAME}-${GIT_COMMIT_HASH}")
      image.push()
    }
  }

  stage('Publish') {
    HOST = "http://service-proxy.ruguoapp.com/service-proxy/jkdpy-dashboard-svc.infra:8000/api/images/"
    DATA_JSON = "{\"image_name\":\"docker.in.ruguoapp.com/${PROJECT_NAME}:${env.BRANCH_NAME}-${GIT_COMMIT_HASH}\",\"branch_name\":\"${env.BRANCH_NAME}\",\"commit_message\":\"${GIT_COMMIT_MESSAGE}\",\"service_name\":\"${PROJECT_NAME}\"}"
    sh "curl -XPOST '${HOST}' -H 'Content-Type:application/json' --data '${DATA_JSON}' --fail"
  }
}
