pipeline {
    agent any
    stages {
        stage('Build All Modules') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test -Dmaven.test.failure.ignore=true'
            }
        }
        stage('PMD') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }
        stage('JaCoCo') {
            steps {
                sh 'mvn jacoco:report'
            }
        }
        stage('Javadoc') {
            steps {
                sh 'mvn javadoc:javadoc'
            }
        }
        stage('Site') {
            steps {
                sh 'mvn site'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
    }
     post {
        always {
            archiveArtifacts artifacts: '**/target/site/**/*.*', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
            junit '**/target/surefire-reports/*.xml'
        }
     }
}
