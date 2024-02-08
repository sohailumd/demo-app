pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh ' pwd; ls -l; chmod +x gradlew'
                echo 'Running build automation'
                sh 'sudo ./gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('deploy-app') {
            steps {
                script {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'demo-app-server',
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm /home/demouser/demo-app/dist/trainSchedule.zip && unzip /tmp/trainSchedule.zip -d /home/demouser/demo-app/dist/ && sudo /usr/bin/systemctl start train-schedule && ls -l /home/demouser/demo-app/dist/trainSchedule.zip'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}