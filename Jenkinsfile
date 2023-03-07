pipeline {

    agent {
        node(label: 'valaxy')
    }

    stages {

        stage('Hello') {

            steps {

                echo '---------------Building---------------'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo '------------Build complete-------------'
 

            }

        }

    }

}