pipeline{
    agent any
    environment {
        DOCKERHUB_PASS = credentials('Docker@123')
    }
    stages {
        stage("Building the Student Survey Image") {
            steps {
                script {
                    checkout scm
                    
                    sh 'rm -rf *.war'
                    sh 'jar -cvf h1.war -C /SWE645HW2 .'
                    sh 'echo ${BUILD_TIMESTAMP}'
                    sh "docker login -u meghanakancherla -p ${DOCKERHUB_PASS}"
                    sh 'docker build -t meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP} .'

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
                sh 'kubectl set image deployment/node-port container-0=meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP}'
            }
        }
        stage("Deploying to Rancher as load balancer"){
            steps {
                sh 'kubectl set image deployment/loadbalancer-2 container-0=meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP}'
            }
        }
    }
}