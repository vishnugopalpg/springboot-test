
properties([
  pipelineTriggers([
    [$class: 'GitHubPushTrigger']
  ])
])
pipeline {
    agent any

    environment {
        JAVA_HOME = '/opt/java/openjdk'
        MAVEN_HOME = '/var/jenkins_home/apache-maven-3.8.9'
        PATH = "/var/jenkins_home:${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${PATH}"
    }

    stages {
        stage('Print Environment') {
            steps {
                echo "💻 JAVA_HOME: ${env.JAVA_HOME}"
                echo "📦 MAVEN_HOME: ${env.MAVEN_HOME}"
                echo "🛣️ PATH: ${env.PATH}"
                sh 'env | grep JAVA'
                sh 'env | grep MAVEN'
                sh 'env | grep PATH'
            }
        }

        stage('Java Version') {
            steps {
                echo '🛠️ Checking Java version...'
                sh 'java -version'
            }
        }

        stage('Maven Version') {
            steps {
                echo '🛠️ Checking Maven version...'
                sh 'mvn -v'
            }
        }

        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds-pat', url: 'https://github.com/vishnugopalpg/springboot-test.git'
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Running Maven Build...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running Unit Tests...'
                sh 'mvn test'
            }
        }
        stage('Code Coverage') {
            steps {
                echo '📊 Generating JaCoCo Report...'
                sh 'mvn verify'
                archiveArtifacts artifacts: 'target/site/jacoco/index.html', fingerprint: true
            }
        }

        stage('Archive JAR') {
            steps {
                echo '📦 Archiving JAR...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}