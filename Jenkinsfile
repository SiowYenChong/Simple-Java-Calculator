pipeline {
    agent any
    stages{
	    stage('Build with Ant') {
            steps {
                script {
                    env.ANT_HOME = "C:\\apache-ant-1.9.16\\bin"
                    sh "${env.ANT_HOME}\\ant -f C:\\Users\\Clarr\\git\\Simple-Java-Calculator\\build.xml"
                }
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
    }
}


