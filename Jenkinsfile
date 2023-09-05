pipeline {
    agent any
    environment {
        ANT_HOME = "C:/apache-ant-1.9.16"
        BUILD_XML_PATH = "C:/Users/Clarr/git/Simple-Java-Calculator/build.xml"
    }
    stages {
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
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv() {
                        // Execute the SonarQube analysis here
                        sh "${env.ANT_HOME}/bin/ant sonar -Dsonar.projectKey=Simple-Java-Calculator -Dsonar.projectName='Simple-Java-Calculator'"
                    }
                }
            }
        }
    }
}
