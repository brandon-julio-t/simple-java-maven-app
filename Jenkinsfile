node {
    docker.image('maven:3-alpine').inside {
        stage('Build') {
            git url: 'https://github.com/brandon-julio-t/simple-java-maven-app'
            sh 'mvn -B -DskipTests clean package'
        }

        stage('Test') {
            try {
                sh 'mvn test'
            } finally {
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Manual Approval') {
            input 'Lanjutkan ke tahap Deploy?'
        }

        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
            sh 'HEROKU_API_KEY="41faae73-d11f-47e9-aa11-573ef901ce2f" mvn heroku:deploy'
            sleep time: 1, unit: 'MINUTES'
        }
    }
}
