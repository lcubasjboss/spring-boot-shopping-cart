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
                 echo '$MVN_HOME'
             }
        }
        stage ('Build App Code') {
            steps {
                  echo '$MVN_HOME'
                  sh '$MVN_HOME/bin/mvn -B -DskipTests clean package' 
                  sh 'mvn -Dmaven.test.failure.ignore=true install'
                  echo "Build App Code Completed"
             }
        }   
        
        stage ('Unit Test App Code') {
            steps {
                  sh 'mvn test' 
                  echo "Unit Test App Code Completed"                
             }
        }   
        
         stage ('Docker Image Build') {
            steps {
                  sh "/usr/bin/docker build -t accountownerapp:B${BUILD_NUMBER} -f Dockerfile ."
                  echo "Docker Image Build Completed"                
             }
        }   
        
        
               /*
         stage (UnitTest') {
             sh "docker build -t accountownerapp:test-B${BUILD_NUMBER} -f Dockerfile.Integration ."
         }

         stage ('Integration Test') {
            sh "docker-compose -f docker-compose.integration.yml up --force-recreate --abort-on-container-exit"
            sh "docker-compose -f docker-compose.integration.yml down -v" 
         }
               */
       }
}
 
