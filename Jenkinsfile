pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'staging', sshCredentials: [encryptedPassphrase: '{AQAAABAAAAAQe9GPnXBnlWyG19qRG6+LEUMEsLPf3gdsJZYrZfxVY1Q=}', key: '', keyPath: '', username: 'deploy'], transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sudo /usr/bin/systemctl stop train-schedule && sudo rm -rf /opt/train-schedule/* && sudo unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/tmp', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/trainSchedule.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
            }
        }
    }
}
