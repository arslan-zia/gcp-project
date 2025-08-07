
pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/arslan-zia/gcp-project.git'
      }
    }
    stage('SonarQube Scan') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'sonar-scanner'
        }
      }
    }
    stage('OWASP Check') {
      steps {
        sh './owasp-dependency-check.sh'
      }
    }
    stage('Docker Build & Trivy Scan') {
      steps {
        sh 'docker build -t gcr.io/$PROJECT_ID/html-app .'
        sh 'trivy image gcr.io/$PROJECT_ID/html-app'
      }
    }
    stage('Push Image to GCR') {
      steps {
        sh 'docker push gcr.io/$PROJECT_ID/html-app'
      }
    }
    stage('Deploy to GKE with Argo CD') {
      steps {
        sh 'kubectl apply -f helm/'
      }
    }
  }
}
