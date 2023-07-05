pipeline{
    agent any
    environment {
        DOCKERHUB_PASS = credentials('docker')
        RANCHER_PASS = credentials('rancher')
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
                def kubeconfigPath = "cluster1.yaml"
                    
                    // Set the kubeconfig file path as an environment variable
                env.KUBECONFIG = kubeconfigPath
                    
                    // Verify Rancher connection
                sh 'kubectl cluster-info'
                sh "kubectl config get-contexts"
                //sh "kubectl set image deployment/hw2-cluster-deploy container-0=meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP}"
            }
        }
        stage("Deploying to Rancher as load balancer"){
            steps {
                sh "kubectl set image deployment/hw2-cluster-deploy container-0=meghanakancherla/studentsurveyh2:${BUILD_TIMESTAMP}"
            }
        }
    }
}