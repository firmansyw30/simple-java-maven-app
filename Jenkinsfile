node {
    try {
        stage('Build') {
            sh 'mvn -B -DskipTests clean package'
        }

        stage('Test') {
            retry(3) {
                sh 'mvn test'
            }
            //sh 'mvn test'
            //junit 'target/surefire-reports/*.xml'
        }

        stage('Manual Approval') {
            def userInput = input(
                id: 'approval',
                message: 'Lanjutkan ke tahap Deploy?',
                parameters: [
                    choice(name: 'Decision', choices: ['Proceed', 'Abort'], description: 'Pilih "Proceed" untuk melanjutkan atau "Abort" untuk menghentikan pipeline')
                ]
            )

            if (userInput == 'Abort') {
                error("Pipeline dihentikan oleh pengguna.")
            }
        }

        stage('Deploy') {
            sh './jenkins/scripts/deploy.sh'
            echo 'Menunggu 1 menit sebelum pipeline berakhir...'
            sleep 60
        }

    } catch (err) {
        echo "Pipeline gagal: ${err}"
        currentBuild.result = 'FAILURE'
    }
}
