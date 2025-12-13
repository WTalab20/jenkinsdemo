pipeline {
  agent any

  environment {
    NEXUS_URL  = "http://127.0.0.1:52116"
    NEXUS_REPO = "maven-releases"
    CREDS_ID  = "nexus-admin"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build with Maven') {
      steps {
        sh 'mvn clean package -DskipTests'
        sh 'ls -la target'
      }
    }

    stage('Upload JAR to Nexus') {
      steps {
        withCredentials([usernamePassword(credentialsId: env.CREDS_ID, usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
          sh '''
            JAR=$(ls target/*.jar | head -n 1)
            echo "Uploading $JAR"
            curl -u $NEXUS_USER:$NEXUS_PASS --upload-file $JAR \
            $NEXUS_URL/repository/$NEXUS_REPO/$(basename $JAR)
          '''
        }
      }
    }
  }
}

