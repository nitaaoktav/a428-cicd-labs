pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }

    stages {

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }

        stage('Manual Approval') {
            steps {
                script {
                    input message: "Lanjutkan ke tahap Deploy?"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                echo "Aplikasi berjalan... Menunggu selama 1 menit."
                sleep(time: 1, unit: "MINUTES")   // ⏳ 1 MENIT
                echo "Waktu habis! Mematikan aplikasi..."
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
