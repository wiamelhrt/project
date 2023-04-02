pipeline {
    agent any
    stages {
        stage('Start Minikube') {
            steps {
                sh 'New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
'
                sh '$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){ `
  [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine) `
}
'
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
