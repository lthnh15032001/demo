pipeline {
    agent any
    stages {
        stage('BuildDebug') {
            steps {
                script {
                    if (env.GIT_BRANCH != 'master'){
                        echo 'This is debug '
                        sh './gradlew assembleDebug'
                    }
                }
            }
        }
         stage('BuildRelease') { 
            steps {
                script {
                    if (env.GIT_BRANCH == 'master'){
                        echo 'This is master '
                        echo "${env.BRANCH_NAME}"
                        sh './gradlew assembleRelease'
                    }
                }
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/*.apk'
            }
        }
        stage('Upload') {
            steps {
                 withAWS(region:'ap-southeast-1',credentials:'cfbdb86b-1a64-46ff-8d4a-079174dad4a9') {
                    script {
                        def identity=awsIdentity(); 
                        echo "${identity}"
                        echo "${JOB_NAME}-${BUILD_NUMBER}"
                        def attachments = [
                            [
                                text: "I find your lack of faith disturbing! ${JOB_NAME}-${BUILD_NUMBER}" ,
                                fallback: 'Hey, Vader seems to be mad at you.',
                                color: '#ff0000'
                            ]
                        ]

                        slackSend(channel: "#jenkins-notify", attachments: attachments)
                    }
                }
            }
        }
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
    post { 
        failure { 
            steps {
                script {
                        def recipients = ['ltdat@rogo.com.vn', 'tan.nguyenvan@rogo.com.vn'] // Replace with the email addresses of the recipients
                        def recipientList = recipients.join(',')
                        emailext to: recipientList,
                        from: 'jenkins@rogo.com.vn',
                        subject: 'Android SDK Build Failed',
                        body: 'The Android SDK has been built failed.'

                        def attachments = [
                            [
                                text: 'I find your lack of faith disturbing!',
                                fallback: 'Hey, Vader seems to be mad at you.',
                                color: '#ff0000'
                            ]
                        ]

                        slackSend(channel: "#jenkins-notify", attachments: attachments)
                    }
            }
        }
    }
}