pipeline {
    agent none
    stages {
        stage('Compile on Slave 1') {
            agent { label 'slave1' }
            steps {
                git branch: 'main', url: 'https://github.com/Miran-Firdausi/Sample-Maven-App.git'
                
                // Explicitly set JAVA_HOME and PATH
                script {
                    def jdkHome = tool name: 'jdk17', type: 'jdk'
                    def mavenHome = tool name: 'maven-3.8.7', type: 'maven'
                    withEnv(["JAVA_HOME=${jdkHome}", "PATH=${jdkHome}/bin:${mavenHome}/bin:${env.PATH}"]) {
                        sh 'mvn clean compile'
                    }
                }
                
                stash includes: '**/target/**', name: 'compiled-code'
            }
        }

        stage('Test on Slave 2') {
            agent { label 'slave2' }
            steps {
                git branch: 'main', url: 'https://github.com/Miran-Firdausi/Sample-Maven-App.git'
                
                unstash 'compiled-code'

                script {
                    def jdkHome = tool name: 'jdk17', type: 'jdk'
                    def mavenHome = tool name: 'maven-3.8.7', type: 'maven'
                    withEnv(["JAVA_HOME=${jdkHome}", "PATH=${jdkHome}/bin:${mavenHome}/bin:${env.PATH}"]) {
                        sh 'mvn test'
                    }
                }
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
