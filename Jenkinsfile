pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                git url:'https://github.com/shruti2222K/cicdakshat/', branch: "master"
               sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t shruti2110/oct302025project:v1 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push shruti2110/oct302025project:v1'
                }
            }
        }
        
        stage('Deploy to k8s') {
           steps {
               withCredentials([kubeconfigFile(credentialsId: 'kubeconfig-cred', variable: 'KUBECONFIG')]) {
                 sh """
                    kubectl set image deployment/my-app-deployment my-app-container=shruti2110/oct302025project:v1
                    kubectl rollout status deployment/my-app-deployment
                    """
             }
         }
     }
       
  }
}
