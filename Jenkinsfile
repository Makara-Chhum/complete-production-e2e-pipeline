pipeline {
    agent {
        label "jenkins_agent"
    }
    tools {
        maven "Maven3"
        jdk "Java17"
    }

    environment {
        APP_NAME = "complete-production-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "dockerproject321"
        DOCKER_PASS = 'dockerhub'
        DOCKER_NAMESPACE = "makaraksk"
        // IMAGE_NAME = "${DOCKER_NAMESPCE}" + "/" + "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_NAME = "${DOCKER_NAMESPACE}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        // JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")

    }
    
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }

        stage("Checkout from SCM"){
            steps {
                git branch : 'main', credentialsId: 'github', url: 'https://github.com/Makara-Chhum/complete-production-e2e-pipeline'
            }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }
        }
        
        stage("Test Application"){
            steps {
                sh "mvn test"
            }
        }

        stage("Sonarqube Analysis"){
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'Jenkins-SonarQube-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Jenkins-SonarQube-token'
                }
            }

        }

        stage("Build & Push Docker Image to Dockerhub dev") {
            steps {
                script {
                    // build docker image
                    def docker_image
                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image = docker.build ("${IMAGE_NAME}:${IMAGE_TAG}")
                        docker_image.push()
                        docker_image.push("latest")
                    }
                }
            }
        }

        // stage("Trigger CD Pipeline") {
        //     steps {
        //         script {
        //             sh "curl -v -k --user admin:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'https://jenkins.makarabmw.store/job/gitops-complete-pipeline/buildWithParameters?token=gitops-token'"
        //         }
        //     }

        // }
    }
}