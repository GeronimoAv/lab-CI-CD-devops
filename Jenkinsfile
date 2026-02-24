// Pipeline CD - Entrega Continua
// Define los stages para construir y publicar la imagen Docker en DockerHub

pipeline {
  agent any

  // Variables del pipeline
  environment {
    C_DOCKER_USER    = credentials('dockerhub-user')
    C_DOCKER_PASS    = credentials('dockerhub-pass')
    C_IMAGE_NAME     = 'devops-app'
    C_IMAGE_TAG      = 'latest'
    C_REPO_URL       = 'https://github.com/GeronimoAv/lab-CI-CD-devops.git'
    C_DOCKER_REGISTRY = 'docker.io'
  }

  stages {

    // Stage 1: Clonar el repositorio
    stage('Clonar repositorio') {
      steps {
        git branch: 'main', url: "${C_REPO_URL}"
      }
    }

    // Stage 2: Construir la imagen Docker
    stage('Construir imagen Docker') {
      steps {
        sh "docker build -t ${C_IMAGE_NAME}:${C_IMAGE_TAG} ."
      }
    }

    // Stage 3: Publicar la imagen en DockerHub
    stage('Publicar imagen en DockerHub') {
      steps {
        sh "echo ${C_DOCKER_PASS} | docker login ${C_DOCKER_REGISTRY} -u ${C_DOCKER_USER} --password-stdin"
        sh "docker tag ${C_IMAGE_NAME}:${C_IMAGE_TAG} ${C_DOCKER_USER}/${C_IMAGE_NAME}:${C_IMAGE_TAG}"
        sh "docker push ${C_DOCKER_USER}/${C_IMAGE_NAME}:${C_IMAGE_TAG}"
      }
    }

  }

  post {
    success {
      echo 'Pipeline CD ejecutado correctamente'
    }
    failure {
      echo 'Pipeline CD falló'
    }
  }
}
