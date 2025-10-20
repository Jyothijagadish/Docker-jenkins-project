pipeline{

    agent any
    environment{
        DOCKER_IMAGE='akashjyothi/docker-jenkins-project'
    }
    tools{
        maven 'maven'
        jdk 'java-11'
    }
    stages{
        stage('git-checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Jyothijagadish/Docker-jenkins-project.git'
            }
        }
        stage('maven-compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('maven-build'){
            steps{
                sh  'mvn clean install'
            }
        }
        stage('Docker build'){
            steps{
                sh 'docker build -t $DOCKER_IMAGE .' 
            }
        }
        stage('Docker push'){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'Docker', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh 'docker push $DOCKER_IMAGE:latest'
}
                }
            }
        }
        stage('Delpoy to kubernetes'){
            steps{
              withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'kubernetes', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
              sh '''
              echo 'applying deployment file'
              kubectl apply -f deployment.yml
              echo 'applying service file'
              kubectl apply -f svc.yml
              '''
             }  
            }
        }
    }

}
post{
    success{
    echo 'kubernetes deployment successful'
    }
    failure{
     echo 'kubernetes deployment failed'   
   }
}
