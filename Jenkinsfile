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
        stage ('Build App Code') {
            steps {
                  sh '/opt/apache-maven-3.3.9/bin/mvn -B -DskipTests clean package' 
                  sh '/opt/apache-maven-3.3.9/bin/mvn -Dmaven.test.failure.ignore=true install'
                  echo "Build App Code Completed"
             }
        }   
        
        stage ('Unit Test App Code') {
            steps {
                  sh '/opt/apache-maven-3.3.9/bin/mvn test' 
                  echo "Unit Test App Code Completed"                
             }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }   
        
         stage ('Build Docker Image') {
            steps {
                  sh 'sudo /usr/bin/docker build -t shopping-cart:dev -f docker/Dockerfile . '
                  sh 'sudo /usr/bin/docker tag shopping-cart:dev lcubasibm/shopping-cart:latest'
                  echo "Docker Image Build & Tag Completed"                
             }
        }   
        
         stage ('Push Docker Image') {
            steps {
                 withCredentials([
      [$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerHub', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASS'],
  ]){
    
      sh """(
        echo "User: ${DOCKER_HUB_USER}"
        echo "Pass: ${DOCKER_HUB_PASS}"
      )"""
    
    sh "sudo /usr/bin/docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASS}"
    sh 'sudo /usr/bin/docker push lcubasibm/shopping-cart:latest'
    
                //    sh 'sudo /usr/bin/docker login -u lcubasibm -p DockerHub2019.'
                 //   sh 'sudo /usr/bin/docker push lcubasibm/shopping-cart:latest'
                    echo "Push Docker Image Completed"                
                //} 
            }
        }   
        
        stage ('Deploy Docker Image') {
            steps {
             sh 'ssh jenkins@172.31.51.31'
             sh 'sudo /usr/bin/docker pull lcubasibm/shopping-cart:latest' 
             sh 'sudo /usr/bin/docker run -d -p 8070:8070 --name shopping-cart shopping-cart:latest'              
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
 
