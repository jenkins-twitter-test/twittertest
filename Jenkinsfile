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
                echo '------------Build complete-------------'
 

            }

        }

    }

}