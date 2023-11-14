pipeline{
    agent any
    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 15, unit: 'SECONDS')
    }

  environment{
    SF_CONSUMER_KEY           = "${env.CONNECTED_APP_CONSUMER_KEY_DH}"
    SERVER_KEY_CREDENTALS_ID  = "${env.JWT_CRED_ID_DH}"
    TEST_LEVEL                = 'RunLocalTests'
    PACKAGE_NAME              = '00D2w000001eI5iEAE'
    SF_INSTANCE_URL           = "${env.SFDC_HOST_DH}"
    SF_USERNAME               = "${env.HUB_ORG_DH}"
    PACKAGE_VERSION           = ''
    server_key_file           = credentials("${env.JWT_CRED_ID_DH}")
    key_file_path             = "${env.WORKSPACE_TMP}\\secretFiles\\${env.JWT_CRED_ID_DH}"
    toolbelt                  = tool 'toolbelt'
    hardpath                  = "C:\\Users\\Owner\\JWT\\server.key"

  }

  stages{                                                                               
    stage('Checkout'){
          steps{
            checkout scm
            echo 'checkout scm'
          }
    }

    stage('Authenticate with Salesforce'){
          
      steps{
        withCredentials([file(credentialsId: "${SERVER_KEY_CREDENTALS_ID}", variable: 'serverkey_file')]) {

          bat script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwtkeyfile \"${serverkey_file}\" --set-default --instanceurl ${SF_INSTANCE_URL}"
          //echo "${key_file_path}"
      }
        
  
      }
    }

    stage('Validate Deployment'){
          
      steps{

          bat script: "\"${toolbelt}\" project deploy start --dry-run --target-org ${SF_USERNAME}"
          //echo "${key_file_path}"        
  
      }
    }
  }
}