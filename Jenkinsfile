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
                      withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh '''
                            echo "$PASS" | docker login -u "$USER" --password-stdin
                           '''
        }
    }
}

        
              stage('Deploy to Kubernetes') {
    steps {
        withKubeConfig([credentialsId: 'kube-credentials']) {
            sh '''
              kubectl apply -f k8s/deploymentservice.yaml
              kubectl rollout status deployment/my-deployment
            '''
        }
    }
}

           
         
     
       
  }
}
