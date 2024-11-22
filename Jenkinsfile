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
        stage('Test') {
            steps {
                sh 'echo this is testing'
                sh 'env'
            }
        }
        stage('Deploy') { 
            when {
                expression { env.GIT_BRANCH == "origin/main" } 
            }
            steps { 
                sh 'echo this is deployment' 
            }
        }
        stage('Print Params') {
            steps { 
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"

            } 
        }
        stage('Approval') {
            input{
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
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