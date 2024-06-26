
pipeline {
    agent any

    environment {
        DOCKER_BFLASK_IMAGE = 'keerthana000/pythonimg1' // Assuming this is defined somewhere
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t my-flask .'
                sh "docker tag my-flask $DOCKER_BFLASK_IMAGE"
            }
        }

        stage('Test') {
            steps {
                sh 'docker run my-flask python -m pytest app/tests/'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
                    sh "docker push $DOCKER_BFLASK_IMAGE"
                }
            }
        }
    }

    post {
        always {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                sh 'docker rm -f mypycont || true' // Remove existing container if it exists
                sh 'docker run --name mypycont -d -p 3000:5000 my-flask'
            }

            mail to: "keerthanaks1814@gmail.com",
                 subject: "Notification mail from Jenkins",
                 body: "CI/CD pipeline completed.\n\nCheck the application."
        }
    }
}

             
              
  

                 
    
  
    

  




             
              
  

                 
    
  
    

  

