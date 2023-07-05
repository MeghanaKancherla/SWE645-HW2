pipeline{
    agent any
    environment {
        DOCKERHUB_PASS = credentials('docker')
    }
    stages {
        stage("Building the Student Survey Image") {
            steps {
                script {
                    checkout scm
                    
                    sh 'rm -rf *.war'
                    //sh 'jar -cvf newh2.war -C SWE645HW2 .'
                    sh 'echo ${BUILD_TIMESTAMP}'
                    sh "docker login -u $DOCKERHUB_PASS_USR -p $DOCKERHUB_PASS_PSW"
                    sh "docker build -t meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP} ."

                }
            }
        }
        stage("Pushing Image to DockerHub") {
            steps {
                script {
                    sh 'docker push meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP}'
                }
            }
        }
        stage("Deploying to Rancher as single pod") {
            steps{
                sh 'kubectl set image deployment/hw2-cluster-deploy container-0=meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP}'
            }
        }
        stage("Deploying to Rancher as load balancer"){
            steps {
                sh 'kubectl set image deployment/loadbalancer-2 container-0=meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP}'
            }
        }
    }
}