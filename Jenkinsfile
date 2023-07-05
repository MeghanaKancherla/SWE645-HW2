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
        stage("Authenticate Rancher") {
            steps{
                //sh ""
                //sh "docker run — rm -v /tmp:/root/.rancher/ rancher/cli2 login https://$rancherUrl/v3 — token $rancherApiToken — skip-verify"
                //sh "docker run — rm -v /tmp:/root/.rancher/ rancher/cli2 catalog refresh $rancherCatalogName — wait"
                //sh "docker run — rm -v /tmp:/root/.rancher/ rancher/cli2 app upgrade $rancherAppName $appVersion"
            }
        }
        stage("Deploying to Rancher as single pod") {
            steps{
                sh "echo kubectl config get-contexts"
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