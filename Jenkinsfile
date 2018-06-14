pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {

        stage( "Set up Environment Variables" ) {
            steps{
                script {
                VERSION = "${BUILD_NUMBER}"
                VERSION_TAG="${VERSION}"
        }

        sh "echo 'version: ${VERSION}'"
        sh "echo 'version_tag: ${VERSION_TAG}'"
      }
    }
        stage('Build') {
            steps {
            echo "-----------------------------------------------"
            echo "          Build Jar File                        "
            echo "-----------------------------------------------"
            sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('TagDeployment'){
            steps{
               sh "cd ${WORKSPACE}"
               sh "pwd"
            }
    }
        stage('Deploy') {
            steps {
               sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
