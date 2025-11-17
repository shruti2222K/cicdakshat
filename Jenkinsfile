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
                sh '''
                  # Make sure the YAML exists
                  if [ ! -f src/deploymentservice.yaml ]; then
                    echo "deploymentservice.yaml not found!"
                    exit 1
                  fi

                  kubectl apply -f src/deploymentservice.yaml
                  kubectl get pods -n default
                '''
            }
        }
           
         
     
       
  }
}
