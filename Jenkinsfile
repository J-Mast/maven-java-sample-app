pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                // Run Maven on a Unix agent.
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                // Run Maven on a Unix agent.
                sh 'mvn package'
            }
            post {
                always {
                    junit stdioRetention: '', testResults: 'target/surefire-reports/*.xml'
                  }
                success {
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
        stage('Deploy') {
            steps {
                // Run Maven on a Unix agent.
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']){
                sh 'nohup java -jar -Dserver.port=8001 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar &'
                }
            }

        }
    }
}
