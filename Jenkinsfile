pipeline {
  environment {
    SERVICE_NAME = "${params.SERVICE_NAME}"
    NAMESPACE = "${params.NAMESPACE}"
    BUILD = "${params.BUILD}"
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/001manoj/java_docker_k8s_project.git', branch: 'master', credentialsId: 'gitCreds'])
        sh 'ls -la'
      }
    }

    // stage('Code Compile') {
    //   steps {
    //     script {
    //       echo 'Code Compile stage'
    //       dir("${SERVICE_NAME}") {
    //         sh 'cd $SERVICE_NAME/'
    //         sh 'mvn clean install'
    //         sh "cp target/${SERVICE_NAME}*.jar ."
    //         sh "mv ${SERVICE_NAME}*.jar app.jar"
    //       }
    //     }
    //   }
    // }

    // stage('Building image') {
    //   steps {
    //     script {
    //       dockerImage = docker.build("${SERVICE_NAME}:v${BUILD}")
    //     }
    //   }
    // }

    // stage('Deploy Image') {
    //   steps {
    //     script {
    //       docker.withRegistry('https://registry.hub.docker.com', 'dockerCreds') {  // Ensure credentialsId is correct
    //         dockerImage.push()
    //       }
    //     }
    //   }
    // }
    // Optional stages for K8s deployment and cleanup can be uncommented if needed.
  }
}

