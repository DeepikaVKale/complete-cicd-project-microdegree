pipeline{
    agent any
    stages{
        stage('checkout'){
            steps{
                git branch: 'S3-file-browser', url:'https://github.com/DeepikaVKale/complete-cicd-project-microdegree.git'
            }
        }
        stage('Docker-image-build'){
            steps{
                sh 'docker build -t deepikavkale/streamlit-classification-app:1 .'
            }
            
        }
        stage('Containerisation'){
            steps{
                sh'''
                docker run -it -d --name c-streamlitapp -p 8501:8501 deepikavkale/streamlit-classification-app:1
                '''
            }
        }
        stage('Login to docker hub'){
            steps{
                script{
                    withCredentials ([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')])
                    {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }
        stage('Pushing image to repository'){
            steps{
                sh 'docker push deepikavkale/streamlit-classification-app:1'
            }
        }
    }
}
