pipeline {
  agent any
  stages {
  stage ('Test')
  {
      steps {
           bat './gradlew test'
           archiveArtifacts 'build/test-results/test/*.xml'
           cucumber buildStatus: 'UNSTABLE',
                   reportTitle: 'Report Cucumber',
                   fileIncludePattern: 'target/report.json'
            }
  }

  stage('CodeAnalysis')
  {
      steps {
      withSonarQubeEnv("sonar") {
                     bat './gradlew sonarqube'
            }

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
          bat './gradlew publish'
              }
    }
  stage('Notification')
        {
             steps {
                                 echo "Notification..."
                                 notifyEvents message: 'Build is created with success', token: 'scoocb5lcsogtwlpl8gbtkyitgaqbmdi'

                   }

                   post {
                         success {
                             mail to: "ka_bellali@esi.dz",
                             subject: "Build Succeeded",
                             body: "Build is deployed with success!"

                         }
                         failure {
                             mail to: "ka_bellali@esi.dz",
                             subject: "Build failed",
                             body: " Build is deployed with failure!"
                          }

                   }
        }

    }
}