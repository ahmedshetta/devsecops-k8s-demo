pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //new comment
            }
      }
      stage('Unit Test') {
            steps {
              sh "mvn test"
            }
        }   
      stage('Docker Build and Push') {
            steps {
              sh "printenv"
              sh 'docker build -t ahmedshetta/numeric-app:""$GIT_COMMIT""'
              sh 'docker push ahmedshetta/numeric-app:""$GIT_COMMIT""'
            }
        }     
    
}
}