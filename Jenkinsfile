pipeline {
    agent {
        label "jenkins_agent"
    }
    tools {
        maven "Maven3"
        jdk "Java17"
    }
    
    stages {
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

    }
}