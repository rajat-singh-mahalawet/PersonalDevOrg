pipeline{
    agent any

    options {
      timeout(time: 2, unit: 'HOURS') //Set a timeout period for the Pipeline run, after which Jenkins should abort the Pipeline
      disableConcurrentBuilds() //Disallow concurrent executions of the Pipeline
      timestamps() //Prepend all console output generated by the Pipeline run with the time at which the line was emitted
      buildDiscarder(logRotator(numToKeepStr:'50', artifactNumToKeepStr: '10')) //Persist artifacts and console output for the specific number of recent Pipeline runs

    }

    parameters {

      booleanParam(
          name: 'DELTA_DEPLOY',
          defaultValue: true,
          description: 'If true, perform delta deployment, otherwise perform a full deployment')
      booleanParam(
          name: 'SFDX_CHECK_ONLY',
          defaultValue: true,
          description: 'If true, will validate deployment, but not save to the org')
      choice(
          name: 'SFDX_TEST_LEVEL',
          choices: ['NoTestRun','RunSpecifiedTests','RunLocalTests','RunAllTestsInOrg'],
          description: 'Sets the Apex test level during deployment. The first value is the default.')
      string(
          name: 'DELTA_DEPLOY_PREV_COMMIT_REF',
          defaultValue: "",
          description: 'If not blank, use this commit ref to compare diffs with instead of a previous CI/CD build')
      booleanParam(
          name: 'DELTA_DEPLOY_DISPLAY_DIFF_FILES',
          defaultValue: true,
          description: 'If true, will display the list added/changed files')

    }

  environment{
    SF_CONSUMER_KEY           = "${env.CONNECTED_APP_CONSUMER_KEY_DH}"
    SERVER_KEY_CREDENTALS_ID  = "${env.JWT_CRED_ID_DH}"
    PACKAGE_NAME              = '00D2w000001eI5iEAE'
    SF_INSTANCE_URL           = "${env.SFDC_HOST_DH}"
    SF_USERNAME               = "${env.HUB_ORG_DH}"
    PACKAGE_VERSION           = ''
    server_key_file           = credentials("${env.JWT_CRED_ID_DH}")
    toolbelt                  = tool 'toolbelt'

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

          bat script: "\"${toolbelt}\" force:auth:jwt:grant --client-id ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwt-key-file \"${serverkey_file}\" --set-default --instance-url ${SF_INSTANCE_URL}"        
        }
  
      }
    }

    stage('logout SF Orgs'){
          
      steps{

          bat script: "\"${toolbelt}\" org logout --all --no-prompt"
  
      }
    }

    stage('Validate Deployment - Dry Run'){

      when{
          expression { return params.SFDX_CHECK_ONLY }
      }
          
      steps{

        echo "Validating Deployment" 
        bat script: "\"${toolbelt}\" project deploy start --dry-run --target-org ${SF_USERNAME}"  
  
      }
    }

    stage('Run Apex Tests'){

      when{
          expression { return params.SFDX_TEST_LEVEL != 'NoTestRun' }
      }
          
      steps{

        echo "Run Apex Tests: ${params.SFDX_TEST_LEVEL}" 

        bat script: "\"${toolbelt}\" project deploy start --dry-run --test-level ${params.SFDX_TEST_LEVEL} -w 180 -o ${SF_USERNAME}"       
  
      }
    }

    stage('Deploy'){

      when{
          expression { return params.SFDX_CHECK_ONLY == false }
      }
          
      steps{
          echo "Begin Deployment" 
          bat script: "\"${toolbelt}\" project deploy start -d ${env.WORKSPACE} --target-org ${SF_USERNAME}"
          echo "Deployed"        
  
      }
    }
  }

  post{
    always{
      echo 'Always logout SF Org.'
      bat script: "\"${toolbelt}\" org logout --all --no-prompt"

      echo "Previous commit ${env.GIT_PREVIOUS_SUCCESSFUL_COMMIT}"

    }
    // success {
    //     setBuildStatus("Build succeeded", "SUCCESS");
    // }
    // failure {
    //     setBuildStatus("Build failed", "FAILURE");
    // }
  }
}