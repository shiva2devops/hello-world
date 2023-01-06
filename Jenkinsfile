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
    stage('Cloning Git') {
      steps {
        checkout scmGit(branches: [[name: '*/master']], extensions: [],
                userRemoteConfigs: [[url: 'https://github.com/shiva2devops/hello-world.git']])
      }
    }
    stage('Install') {
        steps {
            sh 'mvn clean install'
            }
        }
    stage('Generate the Artifacts') {
        steps {
         sshPublisher(publishers: [sshPublisherDesc(configName: 'docker', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
         execCommand: 'cd opt/docker/hello-world/dockerdir/', execTimeout: 120000, flatten: false, makeEmptyDirs: false, 
         noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'opt/docker/hello-world/dockerdir/', 
         remoteDirectorySDF: false, removePrefix: 'webapp/target/', sourceFiles: 'webapp/target/*.war')], usePromotionTimestamp: false,
         useWorkspaceInPromotion: false, verbose: false)])
        }
        }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
