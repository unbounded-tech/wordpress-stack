pipeline {
  agent {
    label "prod"
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '2'))
    disableConcurrentBuilds()
  }
  stages {
    stage("notify") {
      steps {
        slackSend(
          color: "info",
          message: "${env.JOB_NAME} started: ${env.RUN_DISPLAY_URL}"
        )
      }
    }
    stage("setup") {
      when {
        branch "master"
      }
      steps {
        sh "docker network create --driver overlay wordpress || echo 'Network creation failed. It probably already exists.'"
      }
    }
    stage("deploy") {
      when {
        branch "master"
      }
      environment {
        WORDPRESS_DOMAIN = "${env.wordpressDomain}"
        DB_NAME = "${env.wordpressDbName}"
      }
      steps {
        script {
          if (env.wordpressDomain) {
            withCredentials([usernamePassword(
              credentialsId: "wordpress-db-root-auth",
              usernameVariable: "DB_ROOT_USER",
              passwordVariable: "DB_ROOT_PASSWORD"
            )]) {
              withCredentials([usernamePassword(
                credentialsId: "wordpress-db-auth",
                usernameVariable: "DB_USER",
                passwordVariable: "DB_PASSWORD"
              )]) {
                sh "docker stack deploy -c stack.yml wordpress"
              }
            }
          } else {
            sh 'echo "ERROR: env.wordpressDomain is required." && exit 1'
          }
        }
      }
    }
  }
  post {
    always {
      sh "docker system prune -f"
    }
    failure {
      slackSend(
        color: "danger",
        message: "${env.JOB_NAME} failed: ${env.RUN_DISPLAY_URL}"
      )
    }
    success {
      slackSend(
        color: "good",
        message: "${env.JOB_NAME} succeeded: ${env.RUN_DISPLAY_URL}"
      )
    }
  }
}