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
    SF_USERNAME               = ''
    PACKAGE_VERSION           = ''

    server_key_file = credentials("${env.JWT_CRED_ID_DH}")
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
        echo "${server_key_file}"
      }

    }
  }
}
