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

    stage('Package & Dockerize') {
      steps {
        withMaven(
          // Maven installation declared in the Jenkins "Global Tool Configuration"
          maven: 'apache-maven-3.0.5' ) {
            sh 'mvn -B deploy'
        }
      }
    }
    
  }
}