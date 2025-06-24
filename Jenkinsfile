pipeline {
  agent {
    kubernetes {
      label 'agent-s'
    }
  }
  stages {
    stage('Clone') {
      steps {
        git branch: 'main',
          credentialsId: 'github-home-kops-token',
          url: 'https://github.com/home-kops/k8s-media.git'
      }
    }

    stage('Verify') {
      steps {
        container('jnlp') {
          sh '''
            echo "[DEBUG] PATH: $PATH"
            ls -l /usr/local/bin/
            which helm || echo "[DEBUG] helm is not in PATH"
            which kubectl || echo "[DEBUG] kubectl is not in PATH"
          '''

          sh '''
            helm lint ./jellyfin && \
              helm template ./jellyfin -f ./jellyfin/values.yaml
          '''

          sh '''
            helm lint ./calibre && \
              helm template ./calibre -f ./calibre/values.yaml
          '''
        }
      }
    }
  }
}
