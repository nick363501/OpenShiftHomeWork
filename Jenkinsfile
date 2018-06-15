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
                APP_TAGGED="NicolaisApp_v_${BUILD_TIMESTAMP}_${BUILD_NUMBER}.jar"
                DEPLOY_DIR="/tmp/deploy"
                NEXUS_PROTO = "http"
                NEXUS_HOST = "http://nexus3-rn-nexus.apps.na37.openshift.opentlc.com/repository"
                NEXUS_PORT = "80"


                    // job specificstag
                //APP_ID = props['name']
                GIT_REPO = 'GIT PROJECT URL'
                NEXUS_CREDSID = 'NEXUS'
                NEXUS_REPOSITORY = 'Nicolais_Applications'
                NEXUS_GROUP = ''
                DEPLOY_ENV_TARGET = 'DEV'
                DEPLOY_APP_NAME = 'Nicolais_HomeWork'
                DEPLOY_APP_PROCESS = 'Deploy'
                DEPLOY_COMP_NAME = 'Nicolais_HomeWork_App'
                mvnCmd = "mvn -s ./nexus_openshift_settings.xml"

                //POM
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
                sh "mkdir -p ${DEPLOY_DIR}"
                sh "mv -f ./target/${APPNAME} ./target/NicolaisApp_v_${BUILD_TIMESTAMP}_${BUILD_NUMBER}.jar"
                sh "cp -f ./target/NicolaisApp_v_${BUILD_TIMESTAMP}_${BUILD_NUMBER}.jar /tmp/deploy/NicolaisApp_v_${BUILD_TIMESTAMP}_${BUILD_NUMBER}.jar"
                sh "cp -f ./target/NicolaisApp_v_${BUILD_TIMESTAMP}_${BUILD_NUMBER}.jar ../"
                sh "echo ${APP_TAGGED}"
            }
        }

        stage('NEXUS')
        {
            steps
            {   
            echo "___________________________________________________________________________________________________"
            echo "             Deploy Nexus                                           "
            echo "___________________________________________________________________________________________________"
               //sh "${mvnCmd} deploy -DskipTests=true -DaltDeploymentRepository=nexus::default::http://nexus3-rn-nexus.apps.na37.openshift.opentlc.com/repository/Nicolais_Applications/"
               //sh "cd %{DEPLOY_DIR}"
               sh "pwd"
               nexusArtifactUploader artifacts:
               [[artifactId: APPNAME, classifier: '', file: "./target/${APP_TAGGED}", type: 'jar']],
               credentialsId: NEXUS_CREDSID,
               groupId: NEXUS_GROUP,
               nexusUrl: "$NEXUS_HOST:$NEXUS_PORT",
               nexusVersion: 'nexus3',
               protocol: NEXUS_PROTO,
               repository: NEXUS_REPOSITORY,
               version: VERSION
            }
       }

        stage('Deploy') 
        {
            steps {
            echo "___________________________________________________________________________________________________"
            echo "          Deploy Package                                                           "
            echo "___________________________________________________________________________________________________"
            sh "ls -al  ${DEPLOY_DIR}"
            sh "java -jar ${DEPLOY_DIR}/${APP_TAGGED}"
            }
        }
    }
}
