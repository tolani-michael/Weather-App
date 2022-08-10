pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Get code from SCM'){
            steps{
                git branch: 'master', url: 'https://github.com/tolani-michael/Weather-App.git'
            }
        }

        stage('Build & Push docker image'){
            steps{
                sh 'docker build -t ${USERNAME}/helm .'
                sh 'echo "${PASSWORD}" docker login -u ${USERNAME} --password-stdin'
                sh 'docker push ${USERNAME}/helm'
            }
            
        }


        stage('Deploy to Helm') {
                 steps {
                    withAWS(credentials: 'jenkins-aws', region: 'us-east-1') {
                        sh 'aws eks --region  us-east-1 update-kubeconfig --name project'
                        sh 'cat $KUBECONFIG'
                        sh 'helm upgrade -i weather-app ./weatherapp --values weatherapp/values.yaml'
                    }
                        
                }

            }
           
        }

    post {
        always {
            deleteDir()
        }
    }
}
