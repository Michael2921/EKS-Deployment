pipeline {
    agent any

    // environment {

    // }

    // tools {

    // }

    stages {

        stage('Build App') {
            steps {
                script {
                echo 'Building the application...'
                
                }
              
            }
        }
        stage('Build Image') {
            steps {
                script {
                echo "Building the docker image"
                withCredentials([usernamePassword(credentialsId: 'ecr-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                    echo "waiting.."
                }
                }
       
            }
        }
        stage('Deploy') {
            environment {
                AWS_ACCESS_KEY_ID = credentials('jenkins_aws_access_key_id')
                AWS_SECRET_ACCESS_KEY = credentials('jenkins_aws_secret_access_key')
            }
            steps {
                script {
                    echo 'Deleting phpadmin deployment if already deployed'
                    sh 'envsubst < eks-deployment/phpmyadmin-deployment.yaml | kubectl delete -f -'
                    echo 'Deploying mysql...'
                    sh 'helm install mysql --values helmvalues-mysql.yaml bitnami/mysql'
                    echo 'Deploying phpmyadmin'
                    sh 'envsubst < eks-deployment/phpmyadmin-deployment.yaml | kubectl apply -f -'
                }
             
            }
        }

    }
    

}
