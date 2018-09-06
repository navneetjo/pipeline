pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/navneetjo/customer_nodejs_mysql_working.git']]])
            }

        }
        
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'ls -lrth'
                script {
                    env.UPDATE_NAME = input message: 'New Name is required',
                    parameters: [choice(name: 'can choose the below name :', choices: 'Rahul\nMohan', description: 'Choose "yes" if you want to deploy this build')]
        }
            }
        }

        stage('continue') {
            when {
              environment name: 'UPDATE_NAME', value: 'Rahul'
            }
            steps {
                sh 'echo "aborting... name is ${env.UPDATE_NAME}"'
            }
        }


        stage('Deploy') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
                sh 'echo "successfull build"'
                sh 'echo currentBuild.result'
                sh 'printenv'
            }
        }
    }
}