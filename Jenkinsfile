pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker compose build')
            // powershell(script: 'docker images -a')
            // powershell(script: """
            //      cd azure-vote/
            //      docker images -a
            //      docker build -t jenkins-pipeline .
            //      docker images -a
            //      cd ..
            //      """ )
         }
      }
      stage('Start App') {
         steps {
            sh(script: 'docker compose up -d')
         }
      }
      stage('Run Tests') {
         steps {
            echo "Running tests)"
         }
         post {
            success {
               echo "Tests passed! :)"
            }
            failure {
               echo "Tests failed :("
            }
         }
      }
      stage('Docker Push') {
         steps {
            echo "Running in $WORKSPACE"
            dir("$WORKSPACE/azure-vote"){
            script{
                  docker.withRegistry('https://registry-1.docker.io/v2/','dockerhub'){
                     def image = docker.build('stevesam/jenkins-course')
                     image.push()
                  }
            }
          }
         }
      }
   }
   post {
      always {
         sh(script: 'docker compose down')
      }
   }
}
