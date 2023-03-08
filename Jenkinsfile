pipeline {

    tools{
        maven "Maven3"
    }

    agent {
        node(label: 'valaxy')
    }

    stages {

        stage('Build') {

            steps {

                echo '---------------Building---------------'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo '------------Build completed-------------'
 

            }

        }

        stage('Sonar Analysis'){            
            environment {
                    sonarscanner = tool 'SonarScanner'
                }

            steps{
                echo "testing"
                echo '---------- Sonar Analysis started -----------' 

                withSonarQubeEnv('SonarQubeServer'){
                    //sonar server name in master
                    sh "${sonarscanner}/bin/sonar-scanner"
                }
                echo '---------- Sonar Analysis stopped -----------'                                               
            }
        }
    }
}