pipeline {
    agent any
    stages{
        stage('build project'){
            steps{
                git url:'https://github.com/Sujith-darsi/star-agile-health-care.git', branch: "master"
                sh 'mvn clean package'
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t sujithdarsi/staragileprojecthealthcare:v1 .'
                    sh 'docker images'
                }
            }
        }
         stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker tag sujithdarsi/staragileprojecthealthcare:v1 sujithdarsi/staragileprojecthealthcare'
                    sh 'docker push sujithdarsi/staragileprojecthealthcare:v1'
                }
            }
        }
         
        
     stage('Deploy using k8s') {
            steps {
                sh 'sudo kubectl apply -f kubernetesfile.yml'
                sh 'sudo kubectl get all'
                  
                }
            }
        
    }
}
