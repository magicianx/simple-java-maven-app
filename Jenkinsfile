pipeline {
    parameters {
        string(name: 'LABEL', defaultValue: 'node1', description: 'Jenkins Agent')
    }
    agent {
        docker {
            label '${params.LABEL}'
            image 'maven:3-alpine'
            args '-v /home/sa/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
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
