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
        withCredentials(
          [
            string(credentialsId: 'server1-domain', variable: 'DOMAIN'),
            string(credentialsId: 'nfs1-server', variable: 'NFS_SERVER')
          ]
        ) {
          // Run the deploy script dry-run mode to validate the resources
          sh './tooling/deploy -d'
        }
      }
    }
  }
}
