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
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push shruti2110/oct302025project:v1'
                }
            }
        }
        
        
       
        stage('Deploy to Kubernetes') {
    steps {
        sh '''
          kubectl apply -f cicdakshat/src/deploymentservice.yaml
          kubectl get pods -n default
        '''
    }
}

    }
}
