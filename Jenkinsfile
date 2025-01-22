pipeline {
    agent {
        label "jenkins-agent-key"
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
    }
}