pipeline{
    agent any
    
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
                sh 'docker build -t gunaranjanv/project-8:latest .'
            }
        }
        stage('Containerisation'){
            steps{
                sh '''
                docker run -it -d --name c13 -p 9004:8080 gunaranjanv/project-8:latest
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
                sh 'docker push gunaranjanv/project-8:latest'
            }
        }
        
    }
}
