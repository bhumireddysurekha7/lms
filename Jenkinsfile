pipeline {
    agent any
    environment {
        // More detail: 
        // https://jenkins.io/doc/book/pipeline/jenkinsfile/#usernames-and-passwords
        NEXUS_CRED = credentials('nexus')
   }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'cd webapp && npm install && npm run build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'cd webapp && sudo docker container run --rm -e SONAR_HOST_URL="http://18.118.110.136:9000" -e SONAR_TOKEN="sqp_e56ba5ee28021eb669f86622c753939189f20d2b" -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=LMS'
            }
        }
        stage('Release') {
            steps {
                echo 'Release Nexus'
                sh 'rm -rf *.zip'
                sh 'cd webapp && zip dist-${BUILD_NUMBER}.zip -r dist'
                sh 'cd webapp && curl -v -u $Username:$Password --upload-file dist-${BUILD_NUMBER}.zip http://18.118.110.136:8081/repository/LMS/'
            }
        }
    }
}