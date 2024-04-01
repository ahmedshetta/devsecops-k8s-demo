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
      stage('SonarQube SAST Testing'){  
            steps {
              sh "mvn sonar:sonar -Dsonar.projectKey=numeric-application -Dsonar.projectName='numeric-application' -Dsonar.host.url=http://devsecops-demo-se.eastus.cloudapp.azure.com:9000 -Dsonar.token=sqp_1566e16d8aa3440bf08684462fce2b0fec4ba9b0"
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
                sh "sed -i 's#replace#ahmedshetta/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
                sh "kubectl  apply -f k8s_deployment_service.yaml"     
                }
       }
      }
}
}