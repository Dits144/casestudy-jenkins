stage('Deploy to Kubernetes (Helm)') {
  agent {
    docker {
      image 'lachlanevenson/k8s-helm:latest'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  steps {
    withCredentials([file(credentialsId: "${KUBECONFIG_CRED}", variable: 'KUBE_FILE')]) {
      script {
        echo "🚀 Deploying to Kubernetes via Helm..."
        sh '''
          export KUBECONFIG=$KUBE_FILE
          helm upgrade --install $HELM_RELEASE ./helm \
            --set image.repository=$IMAGE \
            --set image.tag=$TAG \
            --namespace $NAMESPACE --create-namespace
        '''
      }
    }
  }
}
