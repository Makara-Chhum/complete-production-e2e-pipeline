pipeline {
    agent {
        label "jenkins-agent"
    }
    tools {
        maven "maven3"
        jdk "java17"
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
    }
}