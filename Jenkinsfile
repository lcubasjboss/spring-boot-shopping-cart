pipeline {
    agent any
    stages {
        stage ('Clone') {
            git url: 'https://github.com/lcubasjboss/spring-boot-shopping-cart.git'
        }
        stage ('Checkout') {
            checkout scm
            echo "Checkout de codigo fuente OK"
        }
        stage ('Build) {
            sh "docker build -t accountownerapp:B${BUILD_NUMBER} -f Dockerfile ."
         }
               /*
         stage (UnitTest') {
             sh "docker build -t accountownerapp:test-B${BUILD_NUMBER} -f Dockerfile.Integration ."
         }

         stage ('Integration Test') {
            sh "docker-compose -f docker-compose.integration.yml up --force-recreate --abort-on-container-exit"
            sh "docker-compose -f docker-compose.integration.yml down -v" */
         }
               */
       }
}
 
