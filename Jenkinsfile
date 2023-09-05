pipeline {
    agent any
    stages{
	    stage('Build with Ant') {
            steps {
                def antHome = tool name: 'Ant', type: 'hudson.tasks.Ant'
                sh "${antHome}/bin/ant -f C:/Users/Clarr/git/Simple-Java-Calculator/build.xml"
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


