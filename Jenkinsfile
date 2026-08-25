pipeline{
  agent {
    label 'dev-server'
  }
  environment{
    DOCKERHUB_USER = 'pallavinielit'
    IMAGE_NAME = 'django-notes-app'
    IMAGE_TAG = 'latest'
  }
  stages{
    stage('Clone the code'){
      steps{
        sh 'whoami'
        git branch: 'main',
          url:'https://github.com/prajapati-pallavi/django-notes-application.git'
      }
    }
    stage('Build Docker Image'){
      steps{
        sh 'docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG .'
      }
    }
    stage('Push Image to Docker Hub'){
      steps{
        withCredentials( [usernamePassword (
          credentialsId: 'dockerHubCreds',
          usernameVariable: 'DH_USER',
          passwordVariable: 'DH_PASS'
        )])

        sh ''' 
        echo $DH_PASS | docker login -u $DH_USER --password-stdin
        docker push $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG
        '''
      }
    }
    stage('Deploy your Application'){
       steps{
          sh '''
          docker compose down || true
          docker compose up -d --build
          '''
      }
    }    
  }
  post{
    always{
      sh 'docker logout || true'
    }
  }

}
