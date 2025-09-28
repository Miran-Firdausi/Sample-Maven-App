pipeline {
    agent none  // no default agent
    stages {
        stage('Compile on Slave 1') {
            agent { label 'slave1' } // assign this stage to slave1
            steps {
                git 'https://github.com/Miran-Firdausi/Sample-Maven-App.git'
                sh 'mvn clean compile'
            }
        }
        stage('Test on Slave 2') {
            agent { label 'slave2' } // assign this stage to slave2
            steps {
                git 'https://github.com/Miran-Firdausi/Sample-Maven-App.git'
                sh 'mvn test'
            }
        }
    }
}
