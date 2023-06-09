pipeline {

    agent any

    environment {
        APP_NAME = 'rschlaravel'
        
        //ENV Production
        PROD_HOST = "${env.PROD_HOST}"
        PROD_USER = "${env.PROD_USER}"
        PROD_AGENT ='jenkins-staging'
        APP_PATH_PROD = '/var/www/html/rschlaravel'
        
        //ENV Staging
        STAGING_HOST = "${env.HOST-STAGING-PORTLET}"
        STAGING_USER = "${env.USER-STAGING-PORTLET}"
        STAGING_AGENT = 'testing0'
        APP_PATH_STAGING = '/var/www/html/rschlaravel1'
        
        //ENV Slack Notification
        SLACK_TOKEN = '0516f92d-2c00-40b0-ad6b-5e25d2eceeea'
        SLACK_CHANNEL = 'mobile-esign'
    }

    options {
        timeout(time: 1, unit: 'HOURS')
    }

    stages{
        stage('Deliver for staging') {

            when {
                branch 'staging'
            }
             
            steps{
                sshagent(credentials:["$STAGING_AGENT"]){
                    sh 'ssh  -o StrictHostKeyChecking=no $PROD_USER@$PROD_HOST "cd $APP_PATH_STAGING && git pull origin staging"'
                }
            }
        }
        
         stage('Deliver for production') {

            when {
                branch 'master'
            }

            steps{
                sshagent(credentials:["$PROD_AGENT"]){
                    sh 'ssh  -o StrictHostKeyChecking=no $PROD_USER@$PROD_HOST "cd $APP_PATH_PROD && git pull origin master"'
                }
            }
        }
    }
    
    post { 
        success { 
            slackSend channel: "$SLACK_CHANNEL", message: "[$APP_NAME] [$GIT_BRANCH] [$GIT_COMMIT] - Deployed success", color:'good', tokenCredentialId: "$SLACK_TOKEN"
        }
        failure {
            slackSend channel: "$SLACK_CHANNEL", failOnError: true, message: "[$APP_NAME] [$GIT_BRANCH] [$GIT_COMMIT] - Deploy failed ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}consoleText|Open Log>)", color:'danger', tokenCredentialId: "$SLACK_TOKEN"
        }
    }
}git