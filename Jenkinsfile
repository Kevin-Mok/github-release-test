pipeline {
    agent { label 'kie-rhel7-priority' }
    environment {
        GOPATH = "$WORKSPACE/go"
        PATH = "$PATH:$GOPATH/bin"
    }
    stages {
        stage('Release') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'kmok-github-bot', usernameVariable: 'GITHUB_USER', passwordVariable: 'GITHUB_TOKEN')]){
                        sh """
                        echo $PATH
                        go get github.com/github-release/github-release
                        github-release info -u Kevin-Mok-Bot -r github-release-test
                        """
                    }
                }
            }
        }
    }
}
