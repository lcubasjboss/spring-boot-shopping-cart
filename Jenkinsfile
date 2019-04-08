pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                 echo "=========================="
                 echo "Inicializando Clone Stage"
                 echo "=========================="
                 git url: 'https://github.com/lcubasjboss/spring-boot-shopping-cart.git'
                 echo "=========================="
                 echo "Finalizando Clone Stage"
                 echo "=========================="
            }
        }
        stage ('Checkout') {
            steps {
                  echo "=========================="
                  echo "Inicializando Checkout Stage"
                  echo "=========================="
                  checkout scm
                  echo "=========================="
                  echo "Finalizando Checkout Stage"
                  echo "=========================="
             }
        }
        stage ('Build App Code') {
            steps {
                  echo "=================================="
                  echo "Inicializando Build App Code Stage"
                  echo "=================================="
                  sh '/opt/apache-maven-3.3.9/bin/mvn -B -DskipTests clean package' 
                  sh '/opt/apache-maven-3.3.9/bin/mvn -Dmaven.test.failure.ignore=true install'
                  echo "=================================="
                  echo "Finalizando Build App Code Stage"
                  echo "=================================="
             }
        }   
        
        stage ('Unit Test App Code') {
            steps {
                  echo "=================================="
                  echo "Inicializando Build App Code Stage"
                  echo "=================================="
                  sh '/opt/apache-maven-3.3.9/bin/mvn test' 
                  echo "=================================="
                  echo "Finalizando Build App Code Stage"
                  echo "=================================="           
             }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }   
        
         stage ('Build Docker Image') {
            steps {
                  echo "======================================"
                  echo "Inicializando Build Docker Image Stage"
                  echo "======================================"
                  echo "current build number: ${currentBuild.number}"
                  //echo "previous build number: ${currentBuild.previousBuild.getNumber()}"             
                  sh 'sudo /usr/bin/docker build -t shopping-cart:${currentBuild.number} -f docker/Dockerfile . '
                  sh 'sudo /usr/bin/docker tag shopping-cart:${currentBuild.number} lcubasibm/shopping-cart:${currentBuild.number}'
                  sh 'sudo /usr/bin/docker tag shopping-cart:${currentBuild.number} lcubasibm/shopping-cart:latest'
                  echo "======================================"
                  echo "Finalizando Build Docker Image Stage"
                  echo "======================================"              
             }
        }   
        
         stage ('Push Docker Image') {
            steps {
                 echo "======================================"
                 echo "Inicializando Push Docker Image Stage"
                 echo "======================================" 
                 withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerHub', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASS'],]){
                 sh """(
                    echo "User: ${DOCKER_HUB_USER}"
                    echo "Pass: ${DOCKER_HUB_PASS}"
                )"""
                sh "sudo /usr/bin/docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASS}"
                sh 'sudo /usr/bin/docker push lcubasibm/shopping-cart:latest'
                echo "======================================"
                echo "Finalizando Push Docker Image Stage"
                echo "======================================" 
                } 
            }
        }   
              
        stage ('Deploy Docker Image to EC2 instance') {
            steps {
                 echo "================================================="
                 echo "Inicializando Deploy Docker Image to EC2 instance"
                 echo "=================================================" 
                                                
                        echo "Deteniendo contendores de shopping-cart"
                        sh 'ssh jenkins@172.31.51.31 "sudo /usr/bin/docker stop $(sudo /usr/bin/docker ps -a | grep shopping-cart | awk '{print $1}')"'
                        sh 'ssh jenkins@172.31.51.31 "sudo /usr/bin/docker rm $(sudo /usr/bin/docker ps -a | grep shopping-cart | awk '{print $1}')"'

                        echo "Borrando Imagen antigua"
                        sh 'ssh jenkins@172.31.51.31 "sudo /usr/bin/docker rmi $(sudo /usr/bin/docker images | grep shopping-cart | awk '{print $3}')"'
                                       
                        echo "Arrancando Contenedor"
                                              
                         sh 'ssh jenkins@172.31.51.31 "sudo /usr/bin/docker pull docker.io/lcubasibm/shopping-cart:latest && sudo /usr/bin/docker run -d -p 8070:8070 --name shopping-cart docker.io/lcubasibm/shopping-cart:latest"'            
                         echo "================================================="
                         echo "Finalizando Deploy Docker Image to EC2 instance"
                         echo "=================================================" 
                         echo " Go to http://jenkinsciserver.tk:8070" 
            }
       }
   }
}
