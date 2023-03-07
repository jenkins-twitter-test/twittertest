pipeline {

    agent {
        node(label: 'valaxy')
    }

    stages {

        stage('Hello') {

            steps {

                echo 'Hello World'
                touch "testfile.txt"

            }

        }

    }

}