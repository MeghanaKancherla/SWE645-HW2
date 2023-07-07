// jenkinsfile
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
                    sh 'jar -cvf newh2.war -C files .'
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
                script {
                    def kubeconfigPath = "cluster1.yaml"
                    env.KUBECONFIG = kubeconfigPath
                    sh "kubectl set image deployment/hw2-cluster-deploy container-0=meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP} -n hw2namespace"
                }
            }
        }
        stage("Deploying to Rancher as load balancer"){
            steps {
                sh "kubectl set image deployment/hw2-cluster-deploy2 container-0=meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP} -n hw2namespace"
                sh "kubectl cluster-info"
            }
        }
    }
}
