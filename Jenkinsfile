pipeline {
  agent {
    docker {
      image "node:10-alpine"
      args "--network=skynet"
    }
  }
  stages {
    stage("Build") {
      steps {
        sh "echo 'http://dl-cdn.alpinelinux.org/alpine/v3.9/main' >> /etc/apk/repositories"
        sh "echo 'http://dl-cdn.alpinelinux.org/alpine/v3.9/community' >> /etc/apk/repositories"
        sh "apk upgrade --update && apk add --no-cache musl\>1.1.20 --repository http://dl-cdn.alpinelinux.org/alpine/edge/main"
        sh "chmod +x ./scripts/dropdb.sh"
        sh "npm install"
      }
    }
    stage("Test") {
      steps {
        sh "npm run test:ci"
      }
      post {
        always {
          junit "log/*.xml"   
        }
      }
    }
  }
}
