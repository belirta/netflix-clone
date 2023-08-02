pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("216780138459.dkr.ecr.us-west-1.amazonaws.com/netflix-app:v1.0.0.RELEASE")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 216780138459.dkr.ecr.us-west-1.amazonaws.com/netflix-app:v1.0.0.RELEASE'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://216780138459.dkr.ecr.us-west-1.amazonaws.com/netflix-app', 'ecr:us-west-1:belirta-ecr') {
                    // build image
                    def myImage = docker.build("216780138459.dkr.ecr.us-west-1.amazonaws.com/netflix-app:v1.0.0.RELEASE")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
