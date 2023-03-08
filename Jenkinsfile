pipeline {

    tools{
        maven "Maven3" //will be installed in slave node
    }

    agent {
        node(label: 'valaxy') // we tell this pipeline to execute all the below commands inside of the slave node
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
                    sonarscanner = tool 'SonarScanner' // sonarscanner will be installed in slave node
                }

            steps{
                echo "testing"
                echo '---------- Sonar Analysis started -----------' 

                withSonarQubeEnv('SonarQubeServer'){
                    //sonar server name in master
                    sh "${sonarscanner}/bin/sonar-scanner"
                }
                echo '---------- Sonar Analysis is successful -----------'                                               
            }
        }
    }
}  