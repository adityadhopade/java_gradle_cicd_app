// def nexus_repository_registry = "34.234.193.66:8083"

pipeline{
  agent any
  environment{
    VERSION = "${env.BUILD_ID}" 
  }
  stages{
    stage("sonar quality check"){
      steps{
        script {
          // need to tell jenkins where sonarqube is hosted for taht need to install plugins
          withSonarQubeEnv(credentialsId: '20sep-sonar-token') {
            sh 'chmod +x gradlew' // change the execute permission
            sh './gradlew sonarqube' // it help to push against sonarqube and validate according to sonar rules
          // remeber to install docer related plugins here
          // if not working as gradle requires java17 to work properly         
          }
          // timeout(time: 1, unit: 'HOURS') {
          //   // Here qg aka Quality Gates represents the qg.status if its okay then move ahead or else throw a message like the following
          //   def qg = waitForQualityGate()
          //   if (qg.status != 'OK') {
          //       error "Pipeline aborted due to quality gate failure: ${qg.status}"
          //   }
          // } 
        }
      }
    }

    stage("Docker Build"){
      steps{
        script{
          withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
          sh '''
            docker build -t 34.234.193.66:8083/spring-app:${VERSION} .
            docker login -u admin -p $docker_password 34.234.193.66:8083
            docker push 34.234.193.66:8083/spring-app:${VERSION}
            docker rmi 34.234.193.66:8083/spring-app:${VERSION}
          '''
          }
        }
      }
    }

    stage("Pushing Helm Charts to Nexus"){
      steps{
        script{
          withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
            dir('kubernetes/') {
              sh '''
                helmversion=$(helm show chart myapp | grep version | cut -d: -f 2 | tr -d ' ')
                tar -czvf myapp-${helm-version}.tgz myapp/ 
                curl -u admin:$docker_password http://34.234.193.66:8081/repository/helm-hosted/ --upload-file myapp-${helmversion}.tgz -v 
              '''
            }
          }
        }
      }
    }
  }
}