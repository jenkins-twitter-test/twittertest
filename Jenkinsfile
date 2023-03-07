pipeline {

    agent {
        node(label: 'valaxy')
    }

    stages {

        stage('Hello') {

            steps {

                echo 'Hello World'
                sh 'touch "createfile.txt"'

            }

        }

    }

}