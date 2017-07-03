pipeline {

  parameters{
    booleanParam(name: 'RELEASE', defaultValue: false, description: 'If this parameter is set, the build will publish the processor as a release ready for production.', )
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }

  agent { 
    node { 
      label 'ci-community-docker' 
    }
  }

  stages {

    stage('Init') {
      steps {
        environment {
          PATH = "${tool 'apache-maven-3.0.5'}/bin:${env.PATH}"
        }
        sh 'mvn -B replacer:replace'
      }
    }

    stage('Package') {
      steps {
        echo "Packaging the application as RPM"
        sh 'mvn -B rpm:rpm'
      }
    }

    stage('Dockerize') {
      steps {
        echo "Packaging the application as Docker"
        sh 'mvn -B docker:build'
      }
    }

    stage('Publish') {
      steps {
        parallel(
          "Publish Manifest": {
            sh 'mvn -B deploy:deploy-file'
          },
          "Push Docker": {
            sh 'mvn -B docker:push'
          }
        )
      }
    }
  }
}