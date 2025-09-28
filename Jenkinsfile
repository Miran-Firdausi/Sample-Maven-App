pipeline {
    agent none
    environment {
        JAVA_HOME = "/opt/java/openjdk"
        PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
    }
    stages {
        stage('Compile on Slave 1') {
            agent { label 'slave1' }
            steps {
                git branch: 'main', url: 'https://github.com/Miran-Firdausi/Sample-Maven-App.git'
                sh 'mvn clean compile test-compile'
                stash includes: '**/target/**', name: 'compiled-code'
            }
        }

        stage('Test on Slave 2') {
            agent { label 'slave2' }
            steps {
                unstash 'compiled-code'
                sh 'java -version'
                sh 'mvn test -DskipCompile'
            }
        }
    }
    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
