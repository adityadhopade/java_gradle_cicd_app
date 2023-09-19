pipeline{
  agent any
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
  }
}