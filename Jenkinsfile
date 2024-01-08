pipeline {
  agent any
  stage ('Test')
  {
      steps {
           bat 'gradlew test'
           junit 'build/test-results/test/*.xml'
           cucumber buildStatus: 'UNSTABLE',
                   reportTitle: 'My report',
                   fileIncludePattern: 'target/report.json'
            }
  stage('CodeAnalysis')
  {
      steps {
      withSonarQubeEnv("sonar") {
                     bat 'gradlew sonarqube' }
            }
  }
  stage('Code Quality')
  {
      steps {
                     timeout(time: 1, unit: 'HOURS') {
                         waitForQualityGate abortPipeline: true
                     }
                 }
  }
  stage('Build')
  {
        steps {
                             bat 'gradlew build'
                             bat 'gradlew javadoc'
                             archiveArtifacts 'build/libs/*.jar'
                             archiveArtifacts 'build/docs/'
                         }
  }
  stage('Deploy')
    {
         steps{
          bat 'gradlew publish'
              }
    }
  stage('Notification')
        {
             steps {
                                 echo "Notification..."
                                 notifyEvents message: 'Build is created with success', token: 'scoocb5lcsogtwlpl8gbtkyitgaqbmdi'
                             }
                         }
                       }


                       post {
                         success {
                             mail to: "ka_bellali@esi.dz",
                             subject: "Build Succeeded",
                             body: "This is an email that informs that the new Build is deployed with success!"
                              slackSend(channel: "#general", message: "Build Succeeded")

                             signalSend message: "Build Succeeded"
                         }
                         failure {
                             mail to: "ka_bellali@esi.dz",
                             subject: "Build failed",
                             body: "This is an email that informs that the new Build is deployed with failure!"
                         }
             }
        }


}