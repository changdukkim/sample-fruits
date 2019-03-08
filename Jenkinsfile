pipeline {
  agent {
      label 'maven'
  }
  stages {
    stage('Build JAR') {
      steps {
        sh "mvn package"
        stash name:"jar", includes:"target/fruits-15-SNAPSHOT.jar"
      }
    }
    stage('Build Image') {
      steps {
        unstash name:"jar"
        script {
          openshift.withCluster() {
            openshift.startBuild("fruits-s2i", "--from-file=target/fruits-15-SNAPSHOT.jar", "--wait")
          }
        }
      }
    }
