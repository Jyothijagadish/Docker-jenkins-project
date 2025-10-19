pipeline{

    agent any

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
                sh  'mv clean install'
            }
        }
    }

}
