pipeline {
    agent any
    environment {
        ANT_HOME = "C:/apache-ant-1.9.16"
        BUILD_XML_PATH = "C:/Users/Clarr/git/Simple-Java-Calculator/build.xml"
    	SONAR_PROJECT_KEY = 'Simple-Java-Calculator'
        SONAR_PROJECT_NAME = 'Simple-Java-Calculator'
        SONAR_HOST_URL = 'http://localhost:9000/'
        SONAR_LOGIN = 'squ_025b7877e89fb15e39f368c2b9eaad3bb9722efa'
        scannerHome = 'C:/sonar-scanner-5.0.1.3006-windows'
    }
    stages {
	    stage('Run Unit Tests') {
            steps {
                dir("C:/Users/Clarr/git/Simple-Java-Calculator") {
                    sh "${env.ANT_HOME}/bin/ant test"
                }
            }
        }
        stage('Build with Ant') {
            steps {
                // Execute the Ant build using the specified build.xml
                sh "${env.ANT_HOME}/bin/ant -f ${env.BUILD_XML_PATH}"
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t siowyenchong/simple-java-calculator .'
                }
            }
        }
        stage('Push image to Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u siowyenchong -p ${dockerhubpwd}'
                    }
                    sh 'docker push siowyenchong/simple-java-calculator'
                }
            }
        }
        stage('Generate SonarQube Analysis') {
            steps {
                script {
                    sh "${env.scannerHome}/bin/sonar-scanner.bat -Dsonar.projectKey=${env.SONAR_PROJECT_KEY} -Dsonar.projectName=${env.SONAR_PROJECT_NAME} -Dsonar.host.url=${env.SONAR_HOST_URL} -Dsonar.login=${env.SONAR_LOGIN}"
                }
            }
        }
        //test Jenkins notification    
    }
    post {
    always {
        script {
            def buildStatus = currentBuild.resultIsBetterOrEqualTo('SUCCESS') ? 'successful' : 'failed'
            def emailBody = "Build ${buildStatus}: ${env.BUILD_URL}"
            emailext body: emailBody,
                     subject: "Build Results (${buildStatus})",
                     to: 'ayj123@1utar.my, anoshaabubakr@1utar.my, chuyan.se@1utar.my, Clairechong998@1utar.my'
        }
    }
}


}
