pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'gradle clean build'
            }
        }

        stage('Test') {
            steps {
                sh 'gradle test'
            }
        }

        stage('Code Coverage') {
            steps {
                sh 'gradle jacocoTestReport'
                jacoco(
                    classPattern: '**/build/classes/java/main',
                    sourcePattern: '**/src/main/java',
                    inclusionPattern: '**/*',
                    exclusionPattern: '**/*Test*',
                    execPattern: '**/build/jacoco/test.exec',
                    changeBuildStatus: true,
                    minimumLineCoverage: '90'
                )
            }
        }

        stage('Deploy') {
            steps {
                sh 'gradle deploy'
            }
        }
    }

    post {
        always {
            junit '**/build/test-results/test/*.xml'
        }

        failure {
            mail to: 'your-email@example.com',
                 subject: 'Build Failed: ${JOB_NAME
