pipeline {
    agent { label 'kie-rhel7-priority' }
    stages {
        stage('Release') {
            steps {
                script {
                    cleanWs()
                    withCredentials([usernamePassword(credentialsId: 'kmok-github-bot', usernameVariable: 'GITHUB_USER', passwordVariable: 'GITHUB_TOKEN')]){
                        sh """
                        go get github.com/github-release/github-release
                        github-release info -u Kevin-Mok-Bot -r github-release-test
                        """
                    }
                }
            }
        }
    }
}
