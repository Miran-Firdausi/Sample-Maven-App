pipeline {
    agent none  // no default agent
    environment { JAVA_HOME = "/opt/java/openjdk" PATH = "${env.JAVA_HOME}/bin:${env.PATH}" }
    stages {
        stage('Compile on Slave 1') {
            agent { label 'slave1' } // assign this stage to slave1
            steps {                
                // Compile the project
                sh 'mvn clean compile'

                // Save compiled files to use in the test stage
                stash includes: '**/target/**', name: 'compiled-code'
            }
        }

        stage('Test on Slave 2') {
            agent { label 'slave2' } // assign this stage to slave2
            steps {
                // Retrieve compiled artifacts from slave1
                unstash 'compiled-code'

                // Run tests
                sh 'mvn test'
            }
        }
    }

    post {
        always {
            // Collect test results in Jenkins
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
