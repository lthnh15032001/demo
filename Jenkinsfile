pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
               sh './gradlew assembleDebug'
            }
        }
         stage('BuildRelease') {
            steps {
               sh './gradlew assembleRelease'
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/*.apk'
            }
        }
        //  stage('Logs') {
        //     steps {
        //         echo 'Do some shitty things' 
        //     }
        // }

        // stage('Test') {
        //     steps {
        //         sh 'mvn test'
        //     }
        //     post {
        //         always {
        //             junit 'target/surefire-reports/*.xml'
        //         }
        //     }
        // }
         stage('Send Email') {
            steps {
                script {
                    def recipients = ['ltdat@rogo.com.vn', 'tan.nguyenvan@rogo.com.vn'] // Replace with the email addresses of the recipients
                    def recipientList = recipients.join(',')
                    emailext to: recipientList,
                    from: 'jenkins@rogo.com.vn',
                    subject: 'Android SDK Build Complete',
                    body: 'The Android SDK has been built and attached to this email.',
                    attachmentsPattern: '**/*.apk'
                }
            }
        }
    }
}