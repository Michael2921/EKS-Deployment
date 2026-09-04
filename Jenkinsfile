pipeline {
    agent any

     environment {
        DOCKER_REPO_SERVER = '420392929933.dkr.ecr.us-east-1.amazonaws.com/company-app'
        IMAGE_NAME = 'test-app'
     }

     tools {
            gradle 'gradle-9.8.0'
            jdk 'jdk-21'
     }

    stages {

        stage('Build App') {
            steps {
                script {
                echo 'Building the application...'
                sh 'gradle build'

                
                }
              
            }
        }
        stage('Build Image') {
            steps {
                script {
                echo "Building the docker image"
                withCredentials([usernamePassword(credentialsId: 'ecr-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                   sh "docker build -t ${DOCKER_REPO_SERVER}:${IMAGE_NAME} ."
                   sh "echo $PASS | docker login -u $USER --password-stdin ${DOCKER_REPO_SERVER} "
                   sh "docker push ${DOCKER_REPO_SERVER}:${IMAGE_NAME} "
                }
                }
       
            }
        }

        // stage('Deploy') {
        //     environment {
        //         AWS_ACCESS_KEY_ID = credentials('jenkins_aws_access_key_id')
        //         AWS_SECRET_ACCESS_KEY = credentials('jenkins_aws_secret_access_key')
        //     }
        //     steps {
        //         script {
        //             echo 'Deploying the configmap'
        //             sh 'envsubst < eks-deployment/companyapp-configmap.yaml | kubectl apply -f -'                    
        //            // echo 'Deploying mysql...'
        //            // sh 'helm repo add bitnami https://charts.bitnami.com/bitnami' // add repo first
        //            // sh 'helm install mysql --values eks-deployment/helmvalues-mysql.yaml bitnami/mysql' // install mysql from added repo
        //             echo 'Deploying phpmyadmin'
        //             sh 'envsubst < eks-deployment/phpmyadmin-deployment.yaml | kubectl apply -f -'
        //             echo 'Deploying companyapp'
        //             sh 'envsubst < eks-deployment/companyapp-deployment.yaml | kubectl apply -f -'
        //         }
             
        //     }
        // }

    }
    

}
