pipeline {
  agent any
  stages {
    stage('Docker login') {
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'nexus_main', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh 'docker login -u $USERNAME -p $PASSWORD ${NEXUS_REGISTRY}'
        }

      }
    }

    stage('buildName') {
      steps {
        script {
          buildName "${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
        }

      }
    }

    stage('Docker build') {
      steps {
        sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ."
      }
    }

    stage('Docker tag') {
      steps {
        sh "docker tag ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ${NEXUS_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
      }
    }

    stage('Docker push') {
      steps {
        sh "docker push ${NEXUS_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
      }
    }

    stage('Docker logout') {
      steps {
        sh "docker logout ${NEXUS_REGISTRY}"
      }
    }

  }
  environment {
    NEXUS_REGISTRY = 'nexus.lazarev.gq'
    DOCKER_IMAGE_NAME = "$JOB_BASE_NAME"
    DOCKER_IMAGE_TAG = "${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
  }
}