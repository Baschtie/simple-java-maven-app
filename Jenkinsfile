pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        archiveArtifacts 'target/*.jar'
      }
    }

    stage('Test') {
      parallel {
        stage('Test') {
          post {
            always {
              junit 'target/surefire-reports/*.xml'
            }

          }
          steps {
            sh 'mvn test'
          }
        }

        stage('Save Artifact') {
          steps {
            archiveArtifacts 'target/*.jar'
          }
        }

      }
    }

    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
      }
    }

  }
}