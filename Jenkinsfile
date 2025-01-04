library identifier: 'Jenkins_Pipelines_shared_libraries@main',
retriever: modernSCM([$class: 'GitSCMSource', 
    remote: 'https://github.com/yaniabdo/Jenkins_Pipelines_shared_libraries.git'])

pipeline {
    agent {
        label 'slave1'
    }
    
    stages {
        stage('Read Parameters') {
            steps {
                script {
                    def json = readJSON file: 'environment.json'
                    env.ENVIRONMENT = json.environment
                }
            }
        }
        
        stage('Shutdown EC2 Instances') {
            steps {
                script {
                    shutdownEC2ByTag(env.ENVIRONMENT)
                }
            }
        }
    }
    
    post {
        success {
            echo "Successfully shutdown EC2 instances for environment: ${env.ENVIRONMENT}"
        }
        failure {
            echo "Failed to shutdown EC2 instances for environment: ${env.ENVIRONMENT}"
        }
    }
}
