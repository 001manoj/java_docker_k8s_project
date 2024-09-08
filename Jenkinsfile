pipeline {
  environment {
    SERVICE_NAME = "${params.SERVICE_NAME}"
    NAMESPACE = "${params.NAMESPACE}"
    BUILD = "${params.BUILD}"
    PATH = "${JAVA_HOME}/bin:${env.PATH}" 
    JAVA_HOME = "/usr/lib/jvm/java-11-openjdk-11.0.24.0.8-3.0.1.el8.x86_64"
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/001manoj/java_docker_k8s_project.git', branch: 'master', credentialsId: 'gitCreds'])
        sh 'ls -la'
      }
    }

    stage('Code Compile') {
      steps {
        script {
          echo 'Code Compile stage'
          dir("${SERVICE_NAME}") {
            sh 'ls -la'
            sh 'mvn clean install'
            sh "cp target/${SERVICE_NAME}*.jar ."
            sh "mv ${SERVICE_NAME}*.jar app.jar"
          }
        }
      }
    }
 

    stage('Building image') {
      steps {
        script {
          dir("${SERVICE_NAME}") {
          dockerImage = docker.build("${SERVICE_NAME}:v${BUILD}")
          }
        }
      }
    }

    stage('Deploy Image') {
      steps {
        script {
          export DOCKER_CLIENT_TIMEOUT=300
          export COMPOSE_HTTP_TIMEOUT=300

          dir("${SERVICE_NAME}") {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerCreds') {  // Ensure credentialsId is correct
            dockerImage.push()
          }
          }
        }
      }
    }
    // Optional stages for K8s deployment and cleanup can be uncommented if needed.
  }
}

