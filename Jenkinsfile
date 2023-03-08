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
                echo '---------- Sonar Analysis started -----------' 

                withSonarQubeEnv('SonarQubeServer'){
                    sh "${sonarscanner}/bin/sonar-scanner"
                }
                echo '---------- Sonar Analysis stopped -----------'                                               
            }
        }
    }
}