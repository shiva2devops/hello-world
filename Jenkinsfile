 pipeline {
    environment { 
        registry = "mshiva7396/project-test" 
        registryCredential = 'docker-hub' 
        dockerImage = '' 
    }
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
        stage('Build Image') {
            steps {
                echo 'Building Docker Image'
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        
        stage('Deploy Image') {
            steps {
                echo 'Pushing Docker Image'
                script {
                   docker.withRegistry( '', registryCredential ) {
                   dockerImage.push("$BUILD_NUMBER")
                   dockerImage.push('latest')
                  }
                }
            }
        }
    }
}
