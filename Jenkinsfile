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
                APPNAME="NicolaisApp-v.jar"
                APPTAGGED="NicolaisApp_v_${BUILD_TIMESTAMP}_${BUILD_NUMBER}.jar"
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
            sh 'mvn -B -DskipTests clean package'
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
                    junit 'target/surefire-reports/*.xml'
                    }
                }
        }

        stage('TagDeployment'){
            steps{
            echo "___________________________________________________________________________________________________"
            echo "             Tag & Rename OpenShift Deployment                                              "
            echo "___________________________________________________________________________________________________"
               sh "cd ${WORKSPACE}"
               sh "pwd"
               echo "${APPNAME}"
                sh "mv -f ./target/${APPNAME} ./target/NicolaisApp_v_${BUILD_TIMESTAMP}_${BUILD_NUMBER}.jar"
                
            }
        }

        stage('Deploy') {
            steps {
            echo "___________________________________________________________________________________________________"
            echo "          Deploy Package                                                           "
            echo "___________________________________________________________________________________________________"
               sh "mkdir -p /tmp/deply"
               sh "cp ./target/${APPTAGGED} /tmp/deploy"
               sh "java -jar /tmp/deploy/${APPTAGGED}"
            }
        }
    }
}
