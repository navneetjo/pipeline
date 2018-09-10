pipeline {
    agent any

    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')

        file(name: "FILE", description: "Choose a file to upload")

        text(name: "Greeting", description: "Choose a name :")

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
                sh 'ls -lrth'
                script {
                    env.UPDATE_NAME = input message: 'New Name is required',
                    parameters: [choice(name: 'can choose the below name :', choices: 'Rahul\nMohan', description: 'Choose "Rahul" if you want to deploy this build')]
                }
            }
        }


        stage('Parameter-outputs') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"

                echo "file uploaded : ${params.FILE}"
            }
        }

        stage('Example') {
            steps {
                echo "${params.Greeting} Hello !"
            }
        }


        stage('continue') {
            when {
              environment name: 'UPDATE_NAME', value: 'Rahul'
            }
            steps {
                sh 'echo "aborting... name is ${UPDATE_NAME}"'
            }

            post {
                always {
                    sh 'echo "always triggers"'
                }
                failure {
                    sh 'echo "this pipeline is in failure state"'
                }
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
                sh 'echo $  currentBuild.result'
                sh 'printenv'
            }
        }


        stage('Stage 1') {
            agent none
            steps {

                timeout(60) {                // timeout waiting for input after 60 minutes
                    script {
                        // capture the approval details in approvalMap.
                        approvalMap = input id: 'test', message: 'Hello', ok: 'Proceed?', parameters: [choice(choices: 'apple\npear\norange', description: 'Select a fruit for this build', name: 'FRUIT'), string(defaultValue: '', description: '', name: 'myparam')], submitter: 'user1,user2,group1', submitterParameter: 'APPROVER'
                    }
                }
            }
        }
        stage('Stage 2') {
            agent any

            steps {
                // print the details gathered from the approval
                echo "This build was approved by: ${approvalMap['APPROVER']}"
                echo "This build is brought to you today by the fruit: ${approvalMap['FRUIT']}"
                echo "This is myparam: ${approvalMap['myparam']}"
            }
        }
        
    }
}