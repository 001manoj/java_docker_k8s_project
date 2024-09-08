

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
 
     stage("Building and Pushing image to dockerHub"){
      steps {
        script {
         
          dir("${SERVICE_NAME}") {
            docker.withRegistry("https://registry.hub.docker.com", 'dockerCreds'){
              def customImage = docker.build("callmemzx/${SERVICE_NAME}:v${BUILD}")
              customImage.push()
              echo "Image has been pushed into dockerHub"
            }
          }
          }
          
        }        
      }

     stage('Deploy to K8s') {
      steps{
        script {
          // sh "sed -i 's,TEST_IMAGE_NAME,harshmanvar/node-web-app:$BUILD_NUMBER,' deployment.yaml"
          sed -i "s|image:.*|image: callmemzx/${SERVICE_NAME}:${BUILD}|g" kubernetes/${SERVICE_NAME}-service.yaml
          sh "kubectl --kubeconfig=/var/lib/jenkins/config apply -f kubernetes/${SERVICE_NAME}-service.yaml"
        }
      }
    }
    //  stage('Building Image') {
    //   steps {
    //     script {
    //       dir("${SERVICE_NAME}") {
    //         dockerImage = docker.build("callmemzx/${SERVICE_NAME}:v${BUILD}")
    //         env.DOCKER_IMAGE = "callmemzx/${SERVICE_NAME}:v${BUILD}"
    //         echo "Built Docker image: ${env.DOCKER_IMAGE}"
    //       }
    //     }
    //   }
    // }

    // stage('Deploy Image') {
    //   steps {
    //     script {
    //       // Docker client timeout settings
    //       withEnv(['DOCKER_CLIENT_TIMEOUT=300', 'COMPOSE_HTTP_TIMEOUT=300']) {
    //         dir("${SERVICE_NAME}") {
    //           echo "Using Docker registry: https://registry.hub.docker.com"
    //           echo "Docker image to push: ${env.DOCKER_IMAGE}"
    //           docker.withRegistry('https://registry.hub.docker.com', 'dockerCreds') {
    //             def image = docker.image(env.DOCKER_IMAGE)
    //             echo "Pushing Docker image: ${env.DOCKER_IMAGE}"
    //             image.push()
    //             echo "Successfully pushed Docker image: ${env.DOCKER_IMAGE}"
    //           }
    //         }
    //       }
    //     }
    //   }
    // }
    // Optional stages for K8s deployment and cleanup can be uncommented if needed.
  }
}
