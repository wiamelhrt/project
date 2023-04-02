pipeline {
    agent any
    stages {
        stage('Start Minikube') {
            steps {
                sh 'minikube start'
            }
        }
        stage('Deploy Kubeflow') {
            steps {
                sh 'kubectl apply -f https://raw.githubusercontent.com/kubeflow/pipelines/master/manifests/kustomize/cluster-scoped-resources/pipelines-default-roles.yaml'
                sh 'kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/dev/?ref=master"'
            }
        }
        stage('Port forward Kubeflow UI') {
            steps {
                sh 'kubectl port-forward -n kubeflow svc/ml-pipeline-ui 8080:80 &'
                script {
                    sleep 10
                }
            }
        }
    }
    post {
        always {
            script {
                sh 'minikube stop'
            }
        }
    }
}
