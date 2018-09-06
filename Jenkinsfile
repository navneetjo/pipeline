def myname
pipeline {
    agent any

    environment {
        MY_ID     = credentials('myid')
    }

    stages {
        stage('Build') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/navneetjo/customer_nodejs_mysql_working.git']]])
            }

        }
        stage('Test') {
            steps {
                echo 'Testing..'
                echo "name is ${myname}"
                echo "ID is $MY_ID"
                sh 'ls -lrth'
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
            }
        }
    }
}