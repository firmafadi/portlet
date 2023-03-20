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
        STAGING_AGENT = ''
        APP_PATH_STAGING = '/var/www/html/rschlaravel1'
        
        //ENV Slack Notification
        SLACK_TOKEN = '0516f92d-2c00-40b0-ad6b-5e25d2eceeea'
        SLACK_CHANNEL = 'mobile-esign'
    }
    stages{
        stage('build') {
           
        }
       
    }
    
   
}