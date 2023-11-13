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
        withCredentials([file(credentialsId: "${SERVER_KEY_CREDENTALS_ID}", variable: 'server_key_file')]) {
              input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?', parameterDefinitions='')
                }
            }
          bat script: "force:auth:jwt:grant --clientid ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwtkeyfile \"${server_key_file}\" --setdefaultdevhubusername --instanceurl ${SF_INSTANCE_URL}"
          echo 'Auth OK'
      }
        
  
      }
    }
  }
}
