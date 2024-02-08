pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh ' pwd; ls -l; rm -rf dist; ls -l; chmod +x gradlew'
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
                                        execCommand: 'mv /tmp/trainSchedule.zip /home/demouser/demo-app/dist/demo.zip'
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