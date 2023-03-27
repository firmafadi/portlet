pipeline {

    agent any

    environment {
        APP_NAME = 'Portlet-APP'
        WORKSPACE = "${env.WORKSPACE}"
        
        //ENV Staging
        STAGING_HOST = "${env.STAGING_HOST}"
        STAGING_USER = "${env.STAGING_USER}"
        STAGING_AGENT = 'ssh-agent-staging'
        APP_PATH_STAGING = '/var/www/portlet'

         //ENV Production
        PROD_HOST = "${env.PROD_HOST}"
        PROD_USER = "${env.PROD_USER}"
        PROD_AGENT ='ssh-agent-prod'
        APP_PATH_PROD = '/var/www-portlet'
        
        //ENV Slack Notification
        SLACK_TOKEN = '0516f92d-2c00-40b0-ad6b-5e25d2eceeea'
        SLACK_CHANNEL = 'slack channel'
    }

    options {
        timeout(time: 1, unit: 'HOURS')
    }
    stages{
        stage('Build') {
            steps{
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Testing') {
            steps{
                sh 'npm run lint'
            }
        }
        stage('Deliver for staging') {

            when {
                branch 'staging'
            }
             
            steps{
                sh 'rsync -a $WORKSPACE/dist $STAGING_USER@$STAGING_HOST:&APP_PATH_STAGING'
            }
        }

        stage('Deliver for staging') {

            when {
                branch 'master'
            }
             
            steps{
                sh 'rsync -a $WORKSPACE/dist $PROD_USER@$PROD_HOST:APP_PATH_PROD'
            }
        }
    }
}