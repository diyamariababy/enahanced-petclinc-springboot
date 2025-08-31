pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment {
        IMAGE_NAME ="springbootapp"
        IMAGE_TAG ="latest"
        ACR_NAME ="jenkinsdiya"
        TENANT_ID ="416a3bf7-5c28-4a75-960a-8a798110fb88"
        ACR_LOGIN_SERVER ="jenkinsdiya.azurecr.io"
    }
   
    stages {
        stage('Checkout From Git') {
            steps {
                git branch: 'prod', url: 'https://github.com/diyamariababy/enahanced-petclinc-springboot.git'
            }
        }
        stage('Maven Validate') {
             steps {
                 echo "This is Maven Validate Stage"
                sh 'mvn validate'
            }
        }  
        stage('Maven Compile') {
            steps {
                echo "This is Maven Compile Stage"
                sh 'mvn compile'
            }
        }
        stage('Sonar Analysis'){
            environment {
                SCANNER_HOME = tool 'SonarScanner'
            }
            steps {
                withSonarQubeEnv('Sonarcloud') {
                    sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.organization=diyamariababy \
                        -Dsonar.projectName=enahanced-petclinc-springboot \
                        -Dsonar.projectKey=diyamariababy_enahanced-petclinc-springboot \
                        -Dsonar.java.binaries=.
                    '''
                }
            }
        } 
        stage('maven package'){
            steps {
                echo "This is maven package"
                sh 'mvn package'
            }
        }
        stage("Docker Build"){
            steps {
                script {
                    echo "Docker Build Started"
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
                
            }
        }
        stage ('ACR LOGIN'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'jenkins-acr-sp', usernameVariable: 'AZURE_USERNAME',passwordVariable: 'AZURE_PASSWORD')]){
                    script {
                        echo "Azure login to container registry"
                        sh '''
                        az login --service-principal -u $AZURE_USERNAME -p $AZURE_PASSWORD --tenant $TENANT_ID
                        az acr login --name $ACR_NAME
                        '''
                    }
                }
            }
        }
    }

}

