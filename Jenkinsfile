pipeline {
    agent any 
	tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }
	
    stages {
        stage('Compile and Clean') { 
            steps {

                sh "mvn clean compile"
            }
        }
        stage('Test') { 
            steps {
                sh "mvn test site"
            }
            
             post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'   
                }
            }     
        }

        stage('deploy') { 
            steps {
                sh "mvn package"
            }
        }


        
        stage('Docker Login'){
            
            steps {
                 
                    sh "docker login -u pankaj134 -p Saini@123}"
                
            }                
        }
		
		

        stage('Docker Push'){
            steps {
                sh 'docker push pankaj134/hello-python:${BUILD_NUMBER}'
            }
        }
        
        stage('Docker deploy'){
            steps {
                sh 'docker run -itd -p 8082:8082 anvbhaskar/hello-python:0.0.3'
            }
        }

        
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}
