pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1') 
    }

    environment {
        MAVEN_HOME = tool 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn clean package'
            }
        }

        stage('Run Tests & Generate Coverage') {
            steps {
                sh '${MAVEN_HOME}/bin/mvn test jacoco:report'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Publish Coverage Report') {
            steps {
                jacoco execPattern: '**/target/jacoco.exec', 
                       classPattern: '**/target/classes', 
                       sourcePattern: '**/src/main/java',
                       exclusionPattern: '**/target/test-classes/**'
            }
        }
    }
}
