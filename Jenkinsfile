pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
               sh './gradlew assembleDebug'
            }
        }
         stage('BuildRelease') {
            when {
                branch 'master'
            }
            steps {
               sh './gradlew assembleRelease'
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

                def identity=awsIdentity();//Log AWS credentials
                echo "${identity}"
                // Upload files from working directory 'dist' in your project workspace
                    // s3Upload(bucket:"yourBucketName", workingDir:'dist', includePathPattern:'**/*');
                s3FindFiles(bucket:'rogo-assets')
                }
            }
        }
         stage('Send Email') {
            steps {
                script {
                    def recipients = ['ltdat@rogo.com.vn', 'tan.nguyenvan@rogo.com.vn', 'lthnh15032001@gmail.com'] // Replace with the email addresses of the recipients
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