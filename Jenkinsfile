pipeline {
  agent {
    docker {
      image 'maven:3.9.9-eclipse-temurin-17'
      args '-v $HOME/.m2:/root/.m2'
    }
  }

  environment {
    NEXUS_URL  = "http://127.0.0.1:52116"
    NEXUS_REPO = "maven-releases"
    CREDS_ID   = "nexus-admin"
  }

  stages {
    stage('Build with Maven') {
      steps {
        sh 'mvn -v'
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
            curl -v -u "$NEXUS_USER:$NEXUS_PASS" --upload-file "$JAR" \
              "$NEXUS_URL/repository/$NEXUS_REPO/$(basename $JAR)"
          '''
        }
      }
    }
  }
}

