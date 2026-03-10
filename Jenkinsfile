pipeline{
    agent any
    environment {
        DOCKER_IMAGE = 'gunaranjanv/project-11:latest'
    }
    tools{
        jdk 'java-11'
        maven 'maven'
    }
    
    stages{
        stage('Git-checkout'){
            steps{
                git branch: 'master' , url: 'https://github.com/GunaranjanV/guna-web.git'
            }
        }
        stage('Code Compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('Code Package'){
            steps{
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Build and tag'){
            steps{
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Containerisation'){
            steps{
                sh '''
                docker run -it -d --name gunaa -p 9016:8080 $DOCKER_IMAGE
                '''
            }
        }
        stage('Login to Docker Hub') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                sh ''' echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin '''
                            }
                        }
                    }
        }
         stage('Pushing image to repository'){
            steps{
                sh 'docker push $DOCKER_IMAGE'
            }
        }
        stage('Deploy to Kubernetes'){
            steps{
                sh 'microk8s.kubectl apply -f deploy.yaml'
            }
        }
    }
}
