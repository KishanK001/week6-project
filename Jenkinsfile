pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
          stage("Compile") {
               steps {
                    sh "chmod +x ./gradlew && ./gradlew compileJava"
               }
          }
          stage("Unit test") {
               when { 
              expression { 
                return env.GIT_BRANCH == "feature" 
              }
           }
               steps {
                    sh "./gradlew test"
               }
          }
          stage("Code coverage") {
               when { 
              expression { 
                return env.GIT_BRANCH == "main" 
              }
           }
               steps {
                    sh "./gradlew jacocoTestReport"
                    sh "./gradlew jacocoTestCoverageVerification"
               }
          }
          stage("Static code analysis") {
               when { 
              expression { 
                return env.GIT_BRANCH == "feature" 
              }
           }
               steps {
                    sh "./gradlew checkstyleMain"
               }
          }
          stage("Package") {
               steps {
                    sh "./gradlew build"
               }
          }

          stage("Update version") {
               steps {
                    sh "sed  -i 's/{{VERSION}}/${BUILD_ID}/g' calculator.yaml"
               }

          }
     }
}
