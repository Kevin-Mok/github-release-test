@Library('jenkins-pipeline-shared-libraries')_

pipeline {
    agent { label 'kie-rhel7-priority' }
    environment {
        GITHUB_REPO = "jenkins-test"
    }
    stages {
        stage('Release') {
            steps {
                script {
                    cleanWs()
                    def version = "7.15.0"
                    checkout(githubscm.resolveRepository(GITHUB_REPO, "Kevin-Mok-Bot", "master", false))
                    withCredentials([usernamePassword(credentialsId: 'kmok-github-bot', usernameVariable: 'GITHUB_USER', passwordVariable: 'GITHUB_TOKEN')]){
                        sh """
                        echo $GOPATH
                        go get github.com/github-release/github-release
                        cp README{,-${version}-linux}.md
                        cp README{,-${version}-darwin}.md
                        github-release release --tag ${version} --name "Jenkins Test ${version}" --description "Testing Jenkins." --pre-release
                        github-release upload --tag ${version} --name "README-${version}-linux.md" --file README-${version}-linux.md
                        github-release upload --tag ${version} --name "README-${version}-darwin.md" --file README-${version}-darwin.md
                        """
                    }
                }
            }
        }
    }
}
