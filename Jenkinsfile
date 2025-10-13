pipeline {
    agent any

    environment {
        PROJECT = "jenkins-pipeline-project"
        YAML_FILE = "html-demo.yaml"
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'main', url: 'https://github.com/asurazoey-code/Pipeline-project.git'
            }
        }

        stage('Deploy to OpenShift') {
            steps {
                sh "oc apply -f ${YAML_FILE} -n ${PROJECT}"
            }
        }

        stage('Rollout Deployment') {
            steps {
                sh "oc rollout latest dc/html-demo -n ${PROJECT} || true"
            }
        }

        stage('Verify Deployment') {
            steps {
                sh "oc get pods -n ${PROJECT}"
                sh "oc get routes -n ${PROJECT}"
            }
        }
    }
}

