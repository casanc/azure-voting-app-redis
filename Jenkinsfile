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
            powershell label:'', script: 'docker images -a'
            powershell label:'', script: """
               cd azure-vote/
               docker images -a
               docker build -t jenkins-pipeline .
               docker images -a
               cd ..
            """
         }
      }
    stage('Stop test app') {
       steps {
          powershell label:'', script: """
             docker-compose down
          """
       }
    }   
   }
}
