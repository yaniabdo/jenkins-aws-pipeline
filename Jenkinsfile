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
                    env.ACTION = json.action.toLowerCase()
                    
                    // Validate action
                    if (!(env.ACTION in ['start', 'stop'])) {
                        error "Invalid action: ${env.ACTION}. Must be either 'start' or 'stop'"
                    }
                }
            }
        }
        
        stage('Manage EC2 Instances') {
            steps {
                script {
                    manageEC2ByTag2(env.ENVIRONMENT, env.ACTION)
                }
            }
        }
    }
    
    post {
        success {
            echo "Successfully ${env.ACTION}ed EC2 instances for environment: ${env.ENVIRONMENT}"
        }
        failure {
            echo "Failed to ${env.ACTION} EC2 instances for environment: ${env.ENVIRONMENT}"
        }
    }
}
