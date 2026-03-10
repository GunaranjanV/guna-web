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
                sh 'docker build -t GunaranjanV/project-5 .'
            }
        }
        stage('Containerisation'){
            steps{
                sh '''
                docker run -it -d --name c9 -p 9009:8080 GunaranjanV/project-5
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
                sh 'docker push GunaranjanV/project-5'
            }
        }
        
    }
}
