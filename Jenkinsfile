 pipeline {
    agent {
        docker {
            image 'maven:3.8.7-eclipse-temurin-11'
            args '-v $HOME/.m2:/root/.m2'
        }
    }
    stages {
        
        stage('git checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [],
                userRemoteConfigs: [[url: 'https://github.com/shiva2devops/hello-world.git']])
            }
        }

        stage('Unit Test & Generate the Artifacts') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('docker Build') {
            agent any
            steps {
                sh 'docker build -t mshiva7396/hellowrld:latest .'
                   }
        }
        stage('docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'docker_pwd', usernameVariable: 'docker_uname')]) {
                    sh "docker login -u ${env.docker_uname} -p ${env.docker_pwd}"
                    sh 'docker push mshiva7396/hellowrld:latest'
   
           }
                   }
        }
        
    }
}
