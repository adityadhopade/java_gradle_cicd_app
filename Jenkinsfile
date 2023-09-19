pipeline{
  agent any
  environment{
    VERSION = "${env.BUILD_ID}" 
  }
  stages{
    stage("sonar quality check"){
      // agent {
      //   docker {
      //     image 'openjdk:17'
      //   }
      // }
      steps{
        script {
          // need to tell jenkins where sonarqube is hosted for taht need to install plugins
          withSonarQubeEnv(credentialsId: 'sonar-token') {
            sh 'chmod +x gradlew' // change the execute permission
            sh './gradlew sonarqube' // it help to push against sonarqube and validate according to sonar rules
          // remeber to install docer related plugins here
          // if not working as gradle requires java17 to work properly         
          }
          timeout(time: 1, unit: 'HOURS') {
            // Here qg aka Quality Gates represents the qg.status if its okay then move ahead or else throw a message like the following
            def qg = waitForQualityGate()
            if (qg.status != 'OK') {
                error "Pipeline aborted due to quality gate failure: ${qg.status}"
            }
          } 
        }
      }
    }

    stage("Docker Build"){
      steps{
        script{
          withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
          sh '''
            sudo chmod 666 /var/run/docker.sock
            docker build -t 54.157.162.169:8083/spring-app:${VERSION} .
            docker login -u admin -p $docker_password 54.157.162.169:8083
            docker push 54.157.162.169:8083/spring-app:${VERSION}
            docker rmi 54.157.162.169:8083/spring-app:${VERSION}
          '''
          }
        }
      }
    }
  }
}