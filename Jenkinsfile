pipeline {
    agent {
        label 'AGENT-1' 
    }
    options{
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds() 
    }
    environment{
        appVersion = ''  // we can use this env variables across the pipeline 
        region = 'us-east-1' 
        account_id = '361769595563'
        project = 'expense'
        environment = 'dev'
        component = 'backend'
    }
    stages {
        stage('Read the version') { 
            steps {
               script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "App version: ${appVersion}" 
               } 
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Docker build') {
            steps { 
                sh """
                aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${account_id}.dkr.ecr.us-east-1.amazonaws.com

                docker build -t ${account_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${environment}/${component}:${appVersion} . 

                docker images

                docker push ${account_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${environment}/${component}:${appVersion}
                """
            }
        }   
    } 
    post{
        always{
            echo "this section runs always" 
            deleteDir() 
        }
        success{
            echo "this section runs when pipeline is success"
        }
        failure{
            echo "this section runs when pipeline is failure" 
        }
    }
} 