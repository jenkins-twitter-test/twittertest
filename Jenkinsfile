def registry = 'https://vpro.jfrog.io'    
def ImageName = 'vpro.jfrog.io/vpro-docker/vproapp'
def Version = '2.0.2'  

pipeline {

    tools{
        maven "Maven3" //will be installed in slave node
    }

    agent {
        node(label: 'valaxy') // we tell this pipeline to execute all the below commands inside of the slave node
    }

    stages {

        stage('Unit tests') {

            steps {
                echo '-------------------Unit testing started------------------------'
                echo "test2"
                // sh 'mvn surefire-report:report'
                echo '-------------Unit testing completed successfully----------------'
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

        stage("Quality Gates"){
            steps{
                script{

                    echo '<--------------- Quality Gate Analysis Started --------------->'
                    timeout(time: 1, unit: 'HOURS'){
                        def qg = waitForQualityGate()
                        if(qg.status !='OK') {
                            error "Pipeline failed due to quality gate failures: ${qg.status}"
                        }
                    }  
                    echo '<--------------- Quality Gate Analysis Ends  --------------->'
                }
            }
        }

        stage('Build') {

            steps {              
                echo '---------------Building---------------'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo '------------Build completed-------------'
            }

        }         
        
        stage("Jar Publish") {

            steps {
                script {
                        echo '<--------------- Jar Publish Started --------------->'
                        def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog-token"
                        def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                        def uploadSpec = """{
                            "files": [
                                {
                                "pattern": "jarstaging/(*)",
                                "target": "twittertest/{1}",
                                "flat": "false",
                                "props" : "${properties}",
                                "exclusions": [ "*.sha1", "*.md5"]
                                }
                            ]
                        }"""
                        def buildInfo = server.upload(uploadSpec)
                        buildInfo.env.collect()
                        server.publishBuildInfo(buildInfo)
                        echo '<--------------- Jar Publish Ended --------------->'  
                
                }
            }   
        }   

        stage('Docker Build') {
            steps{                                
                echo '<--------------- Docker Build Started --------------->'
                dockerImage = docker.build(ImageName + ":" + Version)
                echo '<--------------- Docker Build Ended --------------->'
            }
        }                                
            
    }
}  