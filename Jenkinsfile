pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-west-1'
        AWS_ACCOUNT_ID = '625014114242'
        SNS_TOPIC_NAME = 'test'
    }
    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yassinesah/testrepo'
            }
        }
    }
        stage('Send SNS Notification') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-sns', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    script {
                        def snsTopicArn = "arn:aws:sns:${AWS_REGION}:${AWS_ACCOUNT_ID}:${SNS_TOPIC_NAME}"
                        def message = "A new commit have triggered the job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' ."
                        def messageJson = JsonOutput.toJson([default: message])

                        writeFile file: 'message.json', text: messageJson
                        sh "aws sns publish --region ${AWS_REGION} --topic-arn ${snsTopicArn} --message file://message.json"
                        sh "rm -f message.json"
                    }
                }
            }
        }
}
