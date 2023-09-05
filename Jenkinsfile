pipeline {
    agent any
    environment {
        ANT_HOME = "C:/apache-ant-1.9.16"
        BUILD_XML_PATH = "C:/Users/Clarr/git/Simple-Java-Calculator/build.xml"
    	mvn = "C:/apache-maven-3.9.4"
    }
    stages{
	    stage('Build with Ant') {
            steps {
                sh "${env.ANT_HOME}/bin/ant -f ${env.BUILD_XML_PATH}"
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t siowyenchong/simple-java-calculator .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
	                withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
					    sh 'docker login -u siowyenchong -p ${dockerhubpwd}'
					}
                   sh 'docker push siowyenchong/simple-java-calculator'
                }
            }
        }
        stage('SonarQube Analysis') {
		    withSonarQubeEnv() {
		      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=Simple-Java-Calculator -Dsonar.projectName='Simple-Java-Calculator'"
		    }
		}
    }
}


