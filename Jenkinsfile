pipeline {
    agent {
        node {
            label "node_01"
        }
    }
    triggers { 
        cron('H 2 * * 7')
    }
    stages {
        stage('Clone') {
            steps {
               git url: 'https://github.com/AlexKolyago/speedtest.git',
               credentialsId: "git"
            }
        }
            stage('Test') {
            steps {
                sh """
                nmap -sn 192.168.100.0/24 > speed.txt
                speedtest --accept-license >> speed.txt
                """
            }
        }
        stage('Push') {
            steps {
                sh """
                git add speed.txt
                git config --global user.email "aleksey.kolyago.97@mail.ru"
                git config --global user.name "Alex Kolyago"
                git commit -m "Speed commit"
                git push git@github.com:AlexKolyago/speedtest.git
                """
                
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'speed.txt', onlyIfSuccessful: true
            deleteDir()
        }
    }
}
