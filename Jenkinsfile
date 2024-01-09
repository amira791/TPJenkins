pipeline {
  agent any
  stages {

  stage("test"){
      steps{
         bat './gradlew test'
         archiveArtifacts 'build/reports/'
         cucumber buildStatus: 'UNSTABLE',
         reportTitle: 'My report',
         fileIncludePattern: 'target/report.json'
      }
  }

  stage("code analysis"){
      steps{
        withSonarQubeEnv('sonar') {
             bat './gradlew sonar'
        }
       }
    }
    stage("code quality"){
      steps{
         waitForQualityGate abortPipeline: true
      }
    }

     stage("build"){
        steps{
          bat './gradlew build'
          bat './gradlew javadoc'
          archiveArtifacts 'build/libs/*.jar'
          archiveArtifacts 'build/docs/'
        }
      }

        stage("deploy"){
          steps{
           bat './gradlew publish'
          }
        }

        stage("notification"){
            steps{
              echo "Notification..."
              notifyEvents message: 'Deployment successfully', token: 'scoocb5lcsogtwlpl8gbtkyitgaqbmdi'
            }
        }
}

    post {
            success {

                emailext subject: 'Deployement Succeeded',
                          body: 'This is an email that informs that the new Build is deployed with success!',
                          to: 'soumichan55@gmail.com'

            }
            failure {
                        emailext subject: 'Deployement Failed',
                        body: 'This is an email that informs that the new Build is deployed with failure!',
                        to: 'soumichan55@gmail.com'

            }
        }
}