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
            powershell label:'', script: 'docker rmi -f azure-vote-front'
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
      stage('Run Trivy') {
               steps {
                  sleep(time: 30, unit: 'SECONDS')
                  // pwsh(script: """
                  // C:\\Windows\\System32\\wsl.exe -- sudo trivy blackdentech/jenkins-course
                  // """)
               }
            }
      stage('Start test app') {
         steps {
            powershell label:'', script: """
               docker-compose up -d
               ./scripts/test_container.ps1
            """
         } 
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
      stage('Run Tests') {
         steps {
            powershell label:'', script: """
               pytest ./tests/test_sample.py
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
