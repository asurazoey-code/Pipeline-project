pipeline {
    agent any

    environment {
        OPENSHIFT_SERVER = 'https://api.crc.testing:6443'  // replace with your OpenShift API
        OPENSHIFT_TOKEN  = credentials('OPENSHIFT_TOKEN')
        PROJECT          = 'jenkins-pipeline-project'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/asurazoey-code/Pipeline-project.git'
            }
        }

        stage('Deploy to OpenShift') {
            steps {
                sh """
                oc login ${OPENSHIFT_SERVER} --token=${OPENSHIFT_TOKEN} --insecure-skip-tls-verify=true
                oc project ${PROJECT}
                oc apply -f html-demo.yaml
                oc rollout restart deployment/html-demo
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh """
                oc get pods -n ${PROJECT}
                oc get routes -n ${PROJECT}
                """
            }
        }
    }
}

