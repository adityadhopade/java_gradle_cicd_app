pipeline{
  agent any
  stages{
    stage("sonar quality check"){
      agent {
        docker {
          image 'openjdk:11'
        }
      }
      steps{
        script {
          // need to tell jenkins where sonarqube is hosted for taht need to install plugins
          withSonarQubeEnv(credentialsId: 'sonar-token') {
            sh 'chmod +x gradlew' // change the execute permission
            sh './gradlew sonarqube' // it help to push against sonarqube and validate according to sonar rules
          // remeber to install docer related plugins here
          }
        }
      }
    }
  }
}