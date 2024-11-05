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
                 subject: 'Build Failed: ${JOB_NAME} - #${BUILD_NUMBER}',
                 body: 'Check console output at http://your-jenkins-server/job/${JOB_NAME}/${BUILD_NUMBER}/'
        }
        
        unstable {
            mail to: 'your-email@example.com',
                 subject: 'Build Unstable: ${JOB_NAME} - #${BUILD_NUMBER}',
                 body: 'Check console output at http://your-jenkins-server/job/${JOB_NAME}/${BUILD_NUMBER}/'
        }
        
        success {
            mail to: 'your-email@example.com',
                 subject: 'Build Successful: ${JOB_NAME} - #${BUILD_NUMBER}',
                 body: 'Check console output at http://your-jenkins-server/job/${JOB_NAME}/${BUILD_NUMBER}/'
        }
    }
}