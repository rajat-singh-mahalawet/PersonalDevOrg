pipeline{
    agent any
    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 15, unit: 'SECONDS')
    }

  def SF_CONSUMER_KEY           = env.CONNECTED_APP_CONSUMER_KEY_DH
  def SERVER_KEY_CREDENTALS_ID  = env.JWT_CRED_ID_DH
  def TEST_LEVEL                = 'RunLocalTests'
  def PACKAGE_NAME              = '00D2w000001eI5iEAE'
  def SF_INSTANCE_URL           = env.SFDC_HOST_DH //?: "https://MyDomainName.my.salesforce.com"
  def SF_USERNAME
  def PACKAGE_VERSION

  environment{
    server_key_file = credentials(SERVER_KEY_CREDENTALS_ID)
  }

  stages{                                                                               
    stage('Checkout'){
          steps{
            checkout scm
            echo 'checkout scm'
          }
    }

    stage('Authenticate with Salesforce'){
      echo ${server_key_file}

    }
  }
}
