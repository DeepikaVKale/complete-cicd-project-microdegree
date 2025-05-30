pipeline{
    agent any
    stages{
        stage('checkout'){
            steps{
                git branch 'S3-file-browser', url: 'https://github.com/DeepikaVKale/complete-cicd-project-microdegree.git'
            }
        }
        stage('Docker-image-build'){
            docker build -t cicd-project-microdegree/streamlit-classification-app:1 .
        }
        stage('Containerisation'){
            steps{
                sh'''
                docker run -it -d --name c-streamlitapp -p 8501:8080 cicd-project-microdegree/streamlit-classification-app:1
                '''
            }
        }
        stage('Login to docker hub'){
            steps{
                script{
                    withCredentials ([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')])
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                }
            }
        }
        stage('Pushing image to repository'){
            steps{
                sh 'docker push cicd-project-microdegree/streamlit-classification-app:1'
            }
        }
    }
}