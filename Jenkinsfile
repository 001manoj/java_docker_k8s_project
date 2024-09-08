pipeline {
  environment {
    SERVICE_NAME = "${SERVICE_NAME}"
    NAMESPACE = "${NAMESPACE}"
    BUILD = "${BUILD}"
    // imagename = "harshmanvar/node-web-app"
    // registryCredential = 'docker'
    // dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/001manoj/java_docker_k8s_project.git', branch: 'master', credentialsId: 'gitCreds'])

      }
    }
    stage('Code Compile') {
      steps {
        script {

          echo 'Code Compile stage'

          sh 'mvn  clean  install'
          sh "cp target/${SERVICE_NAME}*.jar  ."
          sh "mv ${SERVICE_NAME}*.jar app.jar"
          }
          
        }
      }
    }

    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build("${SERVICE_NAME}:v${BUILD}")
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', dockerCreds ) {
            dockerImage.push()
             

          }
        }
      }
    }
    // stage('Deploy to K8s') {
    //   steps{
    //     script {
    //       sh "sed -i 's,TEST_IMAGE_NAME,harshmanvar/node-web-app:$BUILD_NUMBER,' deployment.yaml"
    //       sh "cat deployment.yaml"
    //       sh "kubectl --kubeconfig=/home/ec2-user/config get pods"
    //       sh "kubectl --kubeconfig=/home/ec2-user/config apply -f deployment.yaml"
    //     }
    //   }
    // }
    
    
    // stage('Remove Unused docker image') {
    //   steps{
    //     sh "docker rmi $imagename:$BUILD_NUMBER"
    //      sh "docker rmi $imagename:latest"

    //   }
    // }
  }
}
