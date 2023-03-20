pipeline {

    agent any

    environment {
        APP_NAME = 'Testing-App'
        
        //ENV Staging
        STAGING_HOST = "${env.HOST-PORTLET-STAGING}"
        STAGING_USER = "${env.USER-PORTLET-STAGING}"
        STAGING_AGENT = 'ssh-agent-staging'
        APP_PATH_STAGING = '/var/www/html-php56/testing-jenkins'

         //ENV Production
        PROD_HOST = "${env.PROD_HOST}"
        PROD_USER = "${env.PROD_USER}"
        PROD_AGENT ='jenkins-staging'
        APP_PATH_PROD = '/var/www/html/rschlaravel'
        
        //ENV Slack Notification
        SLACK_TOKEN = '0516f92d-2c00-40b0-ad6b-5e25d2eceeea'
        SLACK_CHANNEL = 'slack channel'
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
                    sh 'ssh  -o StrictHostKeyChecking=no $STAGING_USER@$STAGING_HOST "cd $APP_PATH_STAGING && git pull origin staging"'
                }
            }
        }
    }

	// stages{
//         stage('Deliver for production') {

//             when {
//                 branch 'master'
//             }
             
//             steps{
//                 sshagent(credentials:["$PROD_AGENT"]){
//                      withCredentials([gitUsernamePassword(credentialsId: 'idgit', gitToolName: 'Default')]) {
//                          sh 'ssh  -o StrictHostKeyChecking=no $PROD_USER@$PROD_HOST "cd $APP_PATH_PROD && git pull origin //master"'
//                     }
//                     
//                 }
//             }
//         }
//     }
    
}