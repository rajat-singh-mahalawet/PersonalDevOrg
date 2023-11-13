pipeline{
    agent any
    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 15, unit: 'SECONDS')
    }

  stages{
    stage('Checkout'){
          steps{
            checkout scm
            echo 'checkout'
          }
    }
  }
}
