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

      //  stage('Test') {
       //     steps {
        //    echo "_____________________________________________________________________________________________________"
         //   echo "          Test and Publish Junit                                                                     "
         //   echo "_____________________________________________________________________________________________________"
         //       sh 'mvn test'
         //   }
         //   post {
         //       always {
         //           junit 'target/surefire-reports/*.xml'
         //       }
         //   }
        //}

        stage('TagDeployment'){
            steps{
            echo "___________________________________________________________________________________________________"
            echo "             Tag & Rename OpenShift Deployment                                              "
            echo "___________________________________________________________________________________________________"
               sh "cd ${WORKSPACE}"
               sh "cd target"
               sh "pwd"
               echo "${APPNAME}"
                sh "mv -f ${APPNAME} Help"



             

               
            }
    }

        //stage('Deploy') {
          //  steps {
           // echo "___________________________________________________________________________________________________"
           // echo "          Deploy Package                                                           "
           // echo "___________________________________________________________________________________________________"
           //    sh './jenkins/scripts/deliver.sh'
            //}
       // }
    }
}

877 797 5809
