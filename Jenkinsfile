pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                  git url: 'https://github.com/lcubasjboss/spring-boot-shopping-cart.git'
            }
        }
        stage ('Checkout') {
            steps {
                   checkout scm
                  echo "Checkout de codigo fuente OK"
             }
        }
        stage ('Build') {
            steps {
                  sh "docker build -t accountownerapp:B${BUILD_NUMBER} -f Dockerfile ."
             }
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
 
