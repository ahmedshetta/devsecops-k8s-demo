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
              withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                 sh 'printenv'
                 sh 'sudo docker build -t ahmedshetta/numeric-app:""$GIT_COMMIT"" .'
                 sh 'docker push ahmedshetta/numeric-app:""$GIT_COMMIT""'
              }
            }
        }
      stage('Kubernetes Deployment - DEV') {
            steps {
              withKubeConfig([credentialsId: 'kubeconfig']) {
                sh "sed -i 's#replace#ahmedshetta/numeric-app:${GIT_COMMIT}#g' k8s_PROD-deployment_service.yaml"
                sh "kubectl  apply -f k8s_PROD-deployment_service.yaml"     
                }
       }
      }
}
}