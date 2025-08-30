pipeline {
    agent any
    tools {
        maven 'maven'

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
                SCANNER_HOME = tool 'sonarscanner'
            }
            steps {
                withSonarQubeEnv('Sonarcloud') {
                    sh '''
                        $SCANNER_HOME/bin/sonarscanner \
                        -Dsonar.organization=diyamariababy \
                        -Dsonar.projectName=enahanced-petclinc-springboot \
                        -Dsonar.projectKey=diyamariababy_enahanced-petclinc-springboot \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=${sonarcloud-token} \
                        -Dsonar.java.binaries=.
                    '''
                }
            }
        }
    }
}
