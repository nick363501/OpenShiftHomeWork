pipeline {
    agent {
        docker {
            image 'maven:3_alpine'
            args '_v /root/.m2:/root/.m2'
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
            echo "_____________________________________________________________________________________________________"
            echo "          Build Jar File                                                                             "
            echo "_____________________________________________________________________________________________________"
            sh 'mvn _B _DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
            echo "_____________________________________________________________________________________________________"
            echo "          Test and Publish Junit                                                                     "
            echo "_____________________________________________________________________________________________________"
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire_reports/_.xml'
                }
            }
        }

        stage('TagDeployment'){
            steps{
            echo "___________________________________________________________________________________________________"
            echo "             Tag OpenShift Deployment                                              "
            echo "___________________________________________________________________________________________________"
               sh "cd ${WORKSPACE}"
               sh "pwd"
            }
    }

        stage('Deploy') {
            steps {
            echo "___________________________________________________________________________________________________"
            echo "          Deploy Package                                                           "
            echo "___________________________________________________________________________________________________"
               sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
