pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Get code from SCM'){
            steps{
                git branch: 'master', 'https://github.com/tolani-michael/Weather-App.git'
            }
        }

        stage('Build & Push docker image'){
            steps {
                sh 'docker build -t madusonovi/helm .'
                sh 'docker push madusonovi/helm'

            }
        }


        stage('Deploy to Helm') {
                 steps {
                    withKubeConfig([credentialsId: 'jenkins-dev', serverUrl: 'https://B3CDC7A32ECA60F4732BE582C6C86D39.gr7.us-east-1.eks.amazonaws.com']) {
                        sh 'helm install weather-app ./weatherapp --values weatherapp/values.yaml '
                }

            }
           
        }
    }

    post {
        always {
            emailext  (body: "${currentBuild.currentResult}: Job <b>${env.JOB_NAME}</b> completed!! <br><br> More info at: ${env.BUILD_URL}", subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}", to: 'madusonovidius@gmail.com', mimeType: 'text/html');
            deleteDir()
        }
    }
}